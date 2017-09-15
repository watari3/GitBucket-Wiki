### Prerequisites
1. Is installed and running on your server.
2. GitBucket is running on some `port`. The default is `8080`.

### Apache HTTP server set up
1. Install and enable `mod_proxy` and `mod_proxy_http` modules.
2. Add the following to your site configuration (assuming your GitBucket prefix is "gitbucket"):
                
        AllowEncodedSlashes NoDecode # ... or "On" for Apache-2.2
        ProxyPreserveHost On
        ProxyPass /gitbucket http://localhost:8080/gitbucket
        ProxyPassReverse /gitbucket http://localhost:8080/gitbucket

> Note! Apache and GitBucket context paths must be equal:
`java -jar gitbucket.war --port=8080 --prefix=/gitbucket`
(Note the leading / in the prefix)

### or call it up directly from your domain, without /gitbucket folder path, while having it run off a tomcat instance

(i know all this documentation here is horrible, but this should get you to do what you need in case you desire this kind of setup. for the tomcat part, see the wiki entry on tomcat somewhere here)

`/etc/hosts`:
123.123.123.123 this.is.my.domain.tld

apache config example: (make sure the log locations exist and have proper rights)
```
<virtualhost *:80>                                                                                                                                                                                                                                                                 
        servername this.is.my.domain.tld
        
        proxypreservehost on
        proxypass               /       http://localhost:8080/gitbucket/
        proxypassreverse        /       http://localhost:8080/gitbucket/

        customlog       /var/www/this.is.my.domain.tld/logs/access.log common
        errorlog        /var/www/this.is.my.domain.tld/logs/error.log
</virtualhost>
```

gitbucket config: (needed for basepath to make the proxy work actually)
```
#Tue Sep 05 01:41:42 CEST 2017
gravatar=false
notification=false
useSMTP=false
is_create_repository_option_public=true
ldap_authentication=false
ssh=true
allow_account_registration=false
ssh.host=this.is.my.domain.tld
allow_anonymous_access=true
ssh.port=1022
base_url=http\://this.is.my.domain.tld/
```
