Using [Helicontech Zoo](http://www.helicontech.com/articles/deploying-java-servlet-applications-on-windows-with-iis/) we can deploy gitbucket as a java servlet on IIS

* Install Java 7
* Follow these [instructions](http://www.helicontech.com/articles/deploying-java-servlet-applications-on-windows-with-iis/) to install the  Helicontech Zoo Java hosting package
* Configure your gitbucket IIS application folder (ex: C:\inetpub\wwwroot\gitbucket\)
   * add gitbucket.war to C:\inetpub\wwwroot\gitbucket\
   * add web.config to C:\inetpub\wwwroot\gitbucket\

**Contents of web.config, assuming you want to install to http://hostname/gitbucket**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <heliconZoo>
            <clear />
            <application name="gitbucket">
                <environmentVariables>
                    <add name="WAR_EXTRACT_PATH" value="%APPL_PHYSICAL_PATH%" />
                    <add name="WAR_FILE" value="gitbucket.war" />
                    <add name="JDK_HOME" value="C:\Program Files\Java\jdk1.7.0_40" />
                    <add name="JAVA_HOME" value="C:\Program Files\Java\jdk1.7.0_40" />
                    <add name="GITBUCKET_HOME" value="C:\gitbucket_data" />
                    <add name="CONTEXT_PATH" value="/gitbucket" />
                </environmentVariables>
            </application>
    </heliconZoo>
        <handlers>
            <add name="gitbucket#x64" path="*" verb="*" modules="HeliconZoo_x64" scriptProcessor="java.jetty.8" resourceType="Unspecified" requireAccess="Script" preCondition="bitness64" />
            <add name="gitbucket#x86" path="*" verb="*" modules="HeliconZoo_x86" scriptProcessor="java.jetty.8" resourceType="Unspecified" requireAccess="Script" preCondition="bitness32" />
        </handlers>
        <security>
            <requestFiltering>
                <fileExtensions>
                    <remove fileExtension=".ldb" />
                    <remove fileExtension=".refresh" />
                    <remove fileExtension=".webinfo" />
                    <remove fileExtension=".vjsproj" />
                    <remove fileExtension=".vbproj" />
                    <remove fileExtension=".vb" />
                    <remove fileExtension=".resx" />
                    <remove fileExtension=".resources" />
                    <remove fileExtension=".mdf" />
                    <remove fileExtension=".mdb" />
                    <remove fileExtension=".master" />
                    <remove fileExtension=".exclude" />
                    <remove fileExtension=".java" />
                    <remove fileExtension=".csproj" />
                    <remove fileExtension=".config" />
                    <remove fileExtension=".ascx" />
                    <remove fileExtension=".asax" />
                    <remove fileExtension=".cs" />
                </fileExtensions>
            </requestFiltering>
        </security>
  </system.webServer>
</configuration>
```

In the above configuration, some file extensions are removed from request filtering because when running under IIS certain file extensions such as "java", "cs","vb", "csproj","vbproj","ascx", and "asax" will not be viewable in the repository and by default you will get a 404 error.

### Troubleshooting

It should be noted that the docs for [Tomcat with IIS](http://tomcat.apache.org/connectors-doc/reference/iis.html), says the following:

> Note that in a 64 Bit environment - at least for IIS 7 - the used IIS Application Pool should have "Enable 32-bit Applications" set to "False". Otherwise the redirector will not be called and returns an http code 404. If you think, the 32bit version of isapi_redirect.dll would do the job instead, you will get an http code 500, because the library is not loadable into a 64 Bit IIS.

This is a problem that may or may not apply to the Helicontech Zoo, Jetty 8, setup that we use for this  Gitbucket installation or not (I have not thoroughly checked and do not know if this uses the same tech or not).  But if you attach Gitbucket to it's own IIS Application Pool with "Enable 32-bit Applications" set to "False" then Gitbucket does work using Helicontech Zoo and Jetty 8.