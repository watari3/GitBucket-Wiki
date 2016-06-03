Gitbucket uses `slf4j` and `logback` as implementation; thus any logback configuration can apply to gitbucket logging system.

Gitbucket provides its own `logback.xml` (still true in 4.0) preventing external configuration via providing this exact same file.
Logback [official configuration](http://logback.qos.ch/manual/configuration.html) page explain how to override the logging settings:
- provide a file `logback.groovy` at the root of the classpath
- provide a file `logback-test.xml` at the root of the classpath
- provide a file `logback.xml` at the root of the classpath (not usable since gitbucket provides its own)
- provide an implementation of `com.qos.logback.classic.spi.Configurator` via ServiceLoader

Another possibility, as explained in the documentation, is to provide the System property `logback.configurationFile` when launching the application and fill this property with the full path to configuration file.

Using this approach, you could for example launch gitbucket using:
```
java -Dlogback.configurationFile=/opt/gitbucket/config/logback-settings.xml -jar gitbucket.war
```

In the above example, a logback configuration stored inside `/opt/gitbucket/config` would be used, such a file could look like to:
```xml
<configuration debug="true" scan="true" scanPeriod="60 seconds">
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender"> 
    <!-- encoders are  by default assigned the type
         ch.qos.logback.classic.encoder.PatternLayoutEncoder -->
    <filter class="ch.qos.logback.classic.filter.LevelFilter">
      <level>INFO</level>
      <onMatch>ACCEPT</onMatch>
      <onMismatch>DENY</onMismatch>
    </filter>
    <encoder>
            <pattern>ROLLING %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender"> 
    <!-- encoders are  by default assigned the type
         ch.qos.logback.classic.encoder.PatternLayoutEncoder -->
        <file>/opt/gitbucket/log/gitbucket.log</file>
        <append>false</append>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- rollover daily -->
            <fileNamePattern>gitbucket-%d{yyyy-MM-dd}.%i.txt</fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <!-- or whenever the file size reaches 100MB -->
                <maxFileSize>25MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="DEBUG">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="ROLLING"/>
    </root>
</configuration>
``` 

The above configuration:
- activates `DEBUG` traces for all gitbucket code
- outputs the traces on 2 appenders:
  - STDOUT: a console appender printing on stdout by default
  - ROLLING: a file based appender tracing into files stored under `/opt/gitbucket/log/`
- only INFO (and above severity) logs will be printed on the console due to the filter on the `INFO` level
- files in `/opt/gitbucket/log/` will roll daily or each time the file is over 25M

This configuration is just provided as an example, please follow [logback documentation](http://logback.qos.ch/manual/index.html) to setup your own log settings. 