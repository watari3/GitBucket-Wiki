# JBoss/Wildfly

## Jboss 7.X, Wildfly 8.X

For JBoss 7.X, Wildfly 8.X (not tested for 9.X & 10.X) when deploying gitbucket, you will probably face the following deployment error: `java.lang.NoClassDefFoundError: com/sun/net/ssl/internal/ssl/Provider`.

Due to class loading restrictions inside JBoss/Wildfly, you need to explicitly allow gitbucket to access some classes from the jdk. For that, modifiy gitbucket.war and add the following under `WEB-INF\jboss-deployment-structure.xml`

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

## Wildfly 9.X

*TODO*

## Wildfly 10.X

*TODO*


# Tomcat

## Tomcat 7

*TODO*

## Tomcat 8

*TODO*

## Tomcat 9

*TODO*

## Jetty 9

*TODO*

