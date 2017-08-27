# JBoss/Wildfly

## Jboss 7.X, Wildfly 8.X

For JBoss 7.X, Wildfly 8.X (not tested for 9.X & 10.X) when deploying gitbucket, you will probably face the following deployment error: `java.lang.NoClassDefFoundError: com/sun/net/ssl/internal/ssl/Provider`.

Due to class loading restrictions inside JBoss/Wildfly, you need to explicitly allow gitbucket to access some classes from the jdk. For that, modify gitbucket.war and add the following under `WEB-INF\jboss-deployment-structure.xml`

```
<jboss-deployment-structure>
    <deployment>
        <dependencies>
            <system export="true">
                <paths>
                    <path name="com/sun/net/ssl/internal/ssl" />
                    <path name="com/sun/net/ssl" />
                </paths>
            </system>
        </dependencies>
    </deployment>
</jboss-deployment-structure>
```

## Wildfly 9.X, Wildfly 10.X

 Wildfly 9.X & 10.X have the same class loading restrictions as previous versions. So also here you need to explicitly allow gitbucket to access some classes from the jdk. are similar to previous versions. Modify gitbucket.war and add the following under `WEB-INF\jboss-deployment-structure.xml`

```
<jboss-deployment-structure>
  <deployment>
    <dependencies>
      <system export="true">
        <paths>
          <path name="com/sun/net/ssl/internal/ssl" />
          <path name="com/sun/net/ssl" />
        </paths>
      </system>
      <!-- add snakeyaml dependency -->
      <module name="org.yaml.snakeyaml"/>
    </dependencies>
  </deployment>
</jboss-deployment-structure>
```
# Wildfly Install Script
You can use also a install script from [here](https://gist.github.com/rsoika/2d8bab41b18b9fb133c425277c655af7) to install gitbucket on Wildfly 9.x & 10.x on Linux. 

    /bin/bash wildfly-install.sh [INSTALLDIR] [GITBUCKET-VERSION]

You can specify the install directory of your wilfly installation and the gitbucket version. 

Example:

    /bin/bash wildfly-install.sh [INSTALLDIR] [GITBUCKET-VERSION]

**Note:** The script must be run as root. The script assumes that wildfly is running with the user 'wildfly'. You can change this in your script if needed. 

# Tomcat

## Tomcat 7

*TODO*

## Tomcat 8

- install java8
- install tomcat8

   mkdir /srv/gitbucket
   chown tomcat8:tomcat8 /var/gitbucket
   echo "GITBUCKET_HOME=/var/gitbucket" >> /etc/default/tomcat8

- deploy `gitbucket.war` into `/var/lib/tomcat8/webapps
- `service tomcat8 restart`

## Tomcat 9

*TODO*

## Jetty 9

*TODO*

