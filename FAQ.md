## Can I test GitBucket actually before installation?

Access to demo site on heroku: https://gitbucket.herokuapp.com/

## What's default root username and password?

root / root

## Unsupported major.minor version 52.0

GitBucket requires Java8 or higher.

Upgrade your Java installation if you get the following exception:

```
Exception in thread "main" java.lang.UnsupportedClassVersionError: org/eclipse/jetty/server/Handler : Unsupported major.minor version 52.0
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:800)
        at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
        at java.net.URLClassLoader.defineClass(URLClassLoader.java:449)
        ...
```

## How to upgrade GitBucket?

See the [Installation](https://github.com/gitbucket/gitbucket#installation) section of README. 

We strongly recommend to backup your data before upgrade GitBucket.

## How to backup GitBucket data?

Basically, you can backup GitBucket data by copying `GITBUCKET_HOME` directory (`~/.gitbucket` in default) to the other place **after stopping GitBucket**.

See [[Backup]] also to check advanced topics about backup.