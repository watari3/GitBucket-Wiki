# Installation on ubuntu 16.04 with tomcat8 + mysql + proper apache proxy

This is basically copy-paste from http://blog.sjas.de/posts/gitbucket-installation-howto-ubuntu-1604-java8-tomcat8-apache-mysql.html which I wrote for myself. Since I quote myself, it's not stolen. ;)

## goal

On ubuntu 16.04, do:

- install [gitbucket](https://github.com/gitbucket/gitbucket)
- deployed onto a tomcat installation
- with oracle java
- and a mysql backend
- behind an apache proxy
- so its reachable at `http://YOUR.SERVER.NAME`

Should not be too hard, usually.
In reality however, not until you have to work it out from the official documentation, that is.

This was written afterwards from memory and notes and wasn't tested afterwards, in case something doesn't work and you encounter strange behaviours.

The DNS part will not be covered here.
So if you can't handle it yourself, you might need help.
A quick and not-scaling dirty solution would be to put into your workstations hosts file something.

    Linus: `/etc/hosts`
    Windows: `C:\Windows\System32\drivers\etc\hosts` (I think, could be wrong)
    Mac: google yourself where that file lies

Input:

    IP.OF.YOUR.GITSERVER YOUR.SERVER.NAME

## important files / folders and what to do there

    /var/gitbucket                          = gitbucket home
    /var/gitbucket/gitbucket.conf           = set base url here, else proxying will not work
    /var/gitbucket/database.conf            = reference to mysql db
    /var/lib/tomcat8/webapps                    = here goes the gitbucket warfile
    /var/lib/tomcat8/conf/tomcat-users.xml  = just so you have a user for the tomcat guis
    /etc/default/tomcat8                    = set environment variables
    /root/.my.cnf                           = set mysql root credentials for easier working with it

    # some logs you might want to `tail -f` or `multitail` when troubleshooting the installation
    /var/log/mysql/error.log                 = mysql error log, if it doesn't work as expected
    /var/lib/tomcat8/logs/catalina.out       = tomcat general + error log
    /var/log/apache/error.log                = general apache error.log
    /var/www/YOUR.SERVER.NAME/logs/error.log = vhost specific error.log

## installation

    ### fix hostname, if needed

    vim /etc/hostname  ## should only contain the hostname, without domain

    vim /etc/hosts
    ###
    #an entry like this one should be present here:
    aaa.bbb.ccc.ddd YOUR.SERVER.NAME
    ###

### java download, compilation and installation

Head over to [oracle's](http://www.oracle.com/technetwork/java/javase/downloads/index.html) and get your JDK .tar.gz for 64bit linux.
Server JRE should be sufficient, too, but let's not get into that.
Intended version here was 1.8 / Java 8.

Copy the downloaded tar.gz file onto the server, then:

    apt install java-package fakeroot -y
    useradd -m -U -s /bin/bash java_compile_user
    passwd java_compile_user
    mv DOWNLOADED_JAVA.tar.gz /home/java_compile_user
    chown java_compile_user:java_compile_user /home/java_compile_user/DOWNLOADED_JAVA.tar.gz
    su - java_compile_user
    fakeroot make-jpkg DOWNLOADED_JAVA.tar.gz  ## (dont mind the warnings, as long as compilation runs completely through)
    logout
    dpkg -i /home/java_compile_user/GENERATED_JAVA.deb
    userdel -rf java_compile_user

### tomcat8 installation and base configuration

It is assumed, that the server is not externally facing.
Otherwise you should second-guess the ADMIN:ADMIN credentials being set below.

    apt install tomcat8 tomcat8-admin

    cat << EOF > /var/lib/tomcat8/conf/tomcat-users.xml
    <?xml version='1.0' encoding='utf-8'?>
    <tomcat-users xmlns="http://tomcat.apache.org/xml"
                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                  xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
                  version="1.0">

            <user username="ADMIN" password="ADMIN" roles="manager-gui, admin-gui, admin-script" />

    </tomcat-users>
    EOF

    echo 'JAVA_HOME=/usr/lib/jvm/oracle-java8-jdk-amd64' >> /etc/default/tomcat

### mysql installation and base configuration, gitbucket user/database creation

Not all packages are necessary, mytop/mysqltuner/percona-toolkit can be omitted.
Nevertheless, if you work with mysql you should know all these.

How to setup mysqldump will not be covered here.

    touch /var/run/mysqld
    chown mysql:mysql !$
    chmod 0770 /var/run/mysqld
    apt install mysql-server mysql-client mytop mysqltuner percona-toolkit
    cat << EOF > /root/.my.cnf
    [mysql]
    user = root
    password = THE_PASS_YOU_JUST_SET_IN_THE_INSTALLER

    [mysqldump]
    user = root
    password = THE_PASS_YOU_JUST_SET_IN_THE_INSTALLER
    EOF
    mysql  ## test wether login works, if not fix, else you can't continue. Initial mysql setup, if not automated, sucks.
    ## if borked, google how to fix mysql up with a new password in 5.7, that's the version ubuntu 16.04 uses.
    ## otherwise continue at the new mysql prompt

    drop database test;
    create database gitbucket character set utf8 collate utf8_general_ci;
    grant all privileges on `gitbucket`.* to gitbucket@localhost identified by 'YOUR_GITBUCKET_DB_PASSWORD';
    # possibly here at this point you run into some password issues as mysql 5.7 changed user rights management from past handling.
    # if, I can't tell impromptu how to fix it, google it
    flush privileges
    \q

    pt-show-grants  ## if you have percona-toolkit installed
    ###
    #should show something like this:
    -- Grants for 'gitbucket'@'localhost'
    GRANT ALL PRIVILEGES ON `gitbucket`.* TO 'gitbucket'@'localhost';
    GRANT USAGE ON *.* TO 'gitbucket'@'localhost';
    ###
    mysql -u gitbucket -p  ## if everything works, you can login with your new mysql user
    show datases;
    \q

### getting gitbucket, creating data folder, configuring and deploying it

    # Download from https://github.com/gitbucket/gitbucket/releases the release of choice
    # copy onto the server to /var/lib/tomcat8/webapps
    chown tomcat8:tomcat8 /var/lib/tomcat8/webapps/gitbucket.war
    mkdir /var/gitbucket
    chown tomcat8:tomcat8 !$
    chmod 0775 /var/gitbucket
    echo GITBUCKET_HOME=/var/gitbucket >> /etc/default/tomcat8
    cat << EOF > /var/gitbucket/database.conf
    db {
      url = "jdbc:mysql://localhost/gitbucket?useUnicode=true&characterEncoding=utf8"
      user = "gitbucket"
      password = "YOUR_GITBUCKET_DB_PASSWORD"
    }
    EOF

    service tomcat8 restart  ## upon first deployment, the gitbucket.conf gets created, i believe
    cat /var/gitbucket/gitbucket.conf  ## have a look at it. fix base_url and ssh_host, where needed
    # if the base_url setting is wrong, we will not be able to proxy things properly
    # if you run into problems later, check /var/gitbucket/gitbucket.conf - wrong base_url and everything is in vain
    # now check wether you see the tomcat site at http://YOUR.SERVER.NAME:8080
    # with ADMIN:ADMIN you can log into tomcat admin gui, links are in the start page at http://YOUR.SERVER.NAME:8080
    # else fix right issues in files/folders mentioned at beginng that should belong to tomcat8:tomcat8
    cat << EOF > /var/gitbucket/gitbucket.conf
    #Tue Sep 05 01:41:42 CEST 2017
    gravatar=false
    notification=false
    useSMTP=false
    is_create_repository_option_public=true
    ldap_authentication=false
    ssh=true
    allow_account_registration=false
    ssh.host=YOUR.SERVER.NAME
    allow_anonymous_access=true
    ssh.port=1022
    base_url=http\://YOUR.SERVER.NAME
    EOF

### apache, mod_proxy installation, configuration and cookie woes

Login is handled via information in a cookie.
Since we will change the base path of the application when it runs through the proxy, we need to fix the path provided within the cookie so the proxy will work properly (`proxypassreversecookiepath` does this.).
If it is omitted, the site will show up, but you won't be able to login, no matter what you do.

How to set up `logrotate` so your logfile will not run amok, is not covered here.

    apt install apache2
    a2enmod rewrite
    a2enmod headers
    a2enmod proxy
    a2enmod proxy_http

    mkdir -p /var/www/YOUR.SERVER.NAME/logs
    touch /var/www/YOUR.SERVER.NAME/logs/{error,access}.log
    a2dissite *
    service apache2 reload
    cat << EOF > /etc/apache2/sites-available/001-YOUR.SERVER.NAME.conf
    <virtualhost *:80>

            servername YOUR.SERVER.NAME

            proxypreservehost on
            proxypass                       /               http://localhost:8080/gitbucket/
            proxypassreverse                /               http://localhost:8080/gitbucket/
            proxypassreversecookiepath      /gitbucket      /

            customlog       /var/www/YOUR.SERVER.NAME/logs/access.log common
            errorlog        /var/www/YOUR.SERVER.NAME/logs/error.log

    </virtualhost>
    EOF

### get it all up and running (^W^W^W^W^W^Wtroubleshoot the mess)

    #open the logs mentioned at article beginning tailed in other console windows to see what happens when you do these:
    service mysql restart
    service tomcat8 restart
    service apache2 resatrt
    # all this should work, else look what went wrong in the specific log
    # browser: http://YOUR.SERVER.NAME

    # else
    ss -tlpn | cat
    #or
    netstat -tlpn  ## check wether all services run on 80,8080,3306
    iptables -vnxL  ## no rules should be in here to make sure there is not a firewall rule blocking your access.
    # if server is externally facing, fix your firewall, don't just remove it!

Now everything should be running.
If yes, gitbucket will greet you.

Default login for top-right button is `root:root`.
Hope you had fun.
