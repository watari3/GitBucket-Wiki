## Tomcat 8.5 can be used to host GitBucket versions above 3.10 (Java 8)
* Tested on Windows Server 2012 R2 (Version 6.2, Build 9200) with IIS 8.5.9600.16384 
* Install JRE 8
 * [Install Tomcat 8.5 service on Windows Server](http://apache.ip-guide.com/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.exe) - install to `C:\tomcat8`, use default ports
 * [Download Tomcat connector for IIS](https://www.apache.org/dist/tomcat/tomcat-connectors/jk/binaries/windows/tomcat-connectors-1.2.42-windows-x86_64-iis.zip)
   * Copy isapi_redirect.dll from zip to `C:\tomcat8\bin\isapi_redirect.dll`
 * Create Registry entries with Regedit:
```reg
[HKEY_LOCAL_MACHINE\SOFRWARE\Apache Software Foundation\Jakarta Isapi Redirector\1.0]
@=
extension_uri="/jakarta/isapi_redirect.dll"
log_file="C:\tomcat8\logs\isapi_redirect.log"
log_level="error"
worker_file="C:\tomcat8\conf\workers.properties"
worker_mount_file="C:\tomcat8\conf\uriworkermap.properties"
```
* Create file `C:\tomcat8\conf\workers.properties`
```
worker.list = worker1
worker.worker1.host=localhost
worker.worker1.port=8009
worker.worker1.type=ajp13
```
* Create file `C:\tomcat8\conf\uniworkermap.properties`
```
/gitbucket*=worker1
```

* Configure IIS

  * Configure IIS server properties `ISAPI and CGI Restrictions`. Add Path `C:\tomcat8\bin\isapi_redirect.dll` with description `Tomcat Isapi Redirect`.
  * Create VirtualDirectory - **MUST** be named `jakarta` and point to `C:\tomcat8\bin`
  * With`jakarta` VirtualDirectory selected open `Handler Mappings`  and choose `Edit Feature Permissions`. Make sure Read, Script, and Execute permissions are checked.
  * Add ISAPI filter to IIS website configuration. Filter Name: `jakarta`, Executable: `C:\tomcat8\bin\isapi_redirect.dll`
  * The used IIS Application Pool should have `Enable 32-bit Applications` set to `False`
  * Under `Request Filtering` for the website hosting the `jakarta` VirtualDirectory.  Request filters for the VirtualDirectory itself do nothing, you must modify the parent website. Remove request filters for *.java, *.cs and *.vb. If you do not remove these filters then files viewed in gitbucket with these extensions will give a 404 error. You may wish to remove additional filters too such as .csproj.

* place gitbucket.war in `C:\tomcat8\webapps`

* Configure system environment variables for `GITBUCKET_HOME=D:\gitbucket_data` and `GITBUCKET_LOG_DIR=D:\gitbucket_data\logs` 

* At this point gitbucket should be available from both http://localhost/gitbucket (via IIS) and http://localhost:8080/gitbucket (direct connect to Tomcat)

If you have trouble with this configuration refer to [Configure IIS 8 and Tomcat Connector ISAPI Filter on Windows Server 2012](https://www.lisenet.com/2016/configure-iis-8-and-tomcat-connector-isapi-filter-on-windows-server-2012/)

## Helicontech Zoo, can be used to host GitBucket versions 3.10 and below (Java 7).
Using [Helicontech Zoo](http://www.helicontech.com/articles/deploying-java-servlet-applications-on-windows-with-iis/) we can deploy GitBucket as a java servlet on IIS

* Use GitBucket versions 3.10 and below, higher versions require Java 8
* Install Java 7
* Follow these [instructions](http://www.helicontech.com/articles/deploying-java-servlet-applications-on-windows-with-iis/) to install the  Helicontech Zoo Java hosting package
* Configure your gitbucket IIS application folder (ex: C:\inetpub\wwwroot\gitbucket\)
   * add gitbucket.war to C:\inetpub\wwwroot\gitbucket\
   * add web.config to C:\inetpub\wwwroot\gitbucket\
     * verify JDK_HOME and JAVA_HOME paths in web.config
   * configure IIS_IUSRS to have write permissions to C:\inetpub\wwwroot\gitbucket\

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
                <hiddenSegments>
                    <remove segment="App_Browsers" />
                    <remove segment="App_WebReferences" />
                    <remove segment="App_LocalResources" />
                    <remove segment="App_GlobalResources" />
                    <remove segment="App_Data" />
                    <remove segment="App_code" />
                    <remove segment="web.config" />
                </hiddenSegments>
            </requestFiltering>
        </security>
  </system.webServer>
</configuration>
```

In the above configuration, some file extensions are removed from request filtering because when running under IIS certain file extensions such as "java", "cs","vb", "csproj","vbproj","ascx", and "asax" will not be viewable in the repository and by default you will get a 404 error.

### Troubleshooting

#### Tomcat with IIS
It should be noted that the docs for [Tomcat with IIS](http://tomcat.apache.org/connectors-doc/reference/iis.html), says the following:

> Note that in a 64 Bit environment - at least for IIS 7 - the used IIS Application Pool should have "Enable 32-bit Applications" set to "False". Otherwise the redirector will not be called and returns an http code 404. If you think, the 32bit version of isapi_redirect.dll would do the job instead, you will get an http code 500, because the library is not loadable into a 64 Bit IIS.

This is a problem that may or may not apply to the Helicontech Zoo, Jetty 8, setup that we use for this  GitBucket installation or not (I have not thoroughly checked and do not know if this uses the same tech or not).  But if you attach GitBucket to its own IIS Application Pool with "Enable 32-bit Applications" set to "False" then GitBucket does work using Helicontech Zoo and Jetty 8.

#### Maximum Allowed Content Length
Under a server of Windows + IIS environment, if you are not able to push large files (normally larger than 30,000,000 bytes) from any client, it could be that IIS is rejecting your requests. 
In this case, you can raise the **Maximum allowed content length** value as [explained in this documentation](https://www.iis.net/configreference/system.webserver/security/requestfiltering/requestlimits).

Note: this could happen even if you are not using Helicontech Zoo or Tomcat as introduced above; a standalone app could also be interfered by IIS.