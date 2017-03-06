## Can I test GitBucket actually before installation?

There are 2 online installations that demo gitbucket capabilities:

- _Official demo site_: https://gitbucket.herokuapp.com/  
This online demo site contains only the official gitbucket plugins.
- _Community demo site_: https://gitbucket-community.herokuapp.com/  
This demo site contains mostly all known plugins ; ie the official ones + the ones delivered by the gitbucket community.

## What's default root username and password?

root / root

## Docker image?

If you have docker host, this command just works(See [docker-gitbucket](https://github.com/f99aq8ove/docker-gitbucket)):

`% docker run -d -p 8080:8080 -p 29418:29418 -v ${PWD}/gitbucket-data:/gitbucket f99aq8ove/gitbucket`

Then you can access via HTTP to http://localhost:8080/, and you can also [[Reverse-proxy-with-Nginx]].

Need plugins? Then you can allocate plugin JARs into `${PWD}/gitbucket-data/plugins/` (create if not exists), and restart the container.

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

## gitbucket-gist-plugin doesn't work after upgrading to GitBucket 4.x

If you had used [gitbucket-gist-plugin](https://github.com/gitbucket/gitbucket-gist-plugin) with GitBucket 3.x, it does not work after upgrading to GitBucket 4.x. Solution is below:

1. `UPDATE VERSIONS SET VERSION='2.0.0' WHERE MODULE_ID='gist';`
2. restart gitbucket
3. can open snippets page
4. `SELECT VERSION FROM VERSIONS WHERE MODULE_ID='gist'` -> `4.2.0`

See [[Connect to H2 database]] to know how to execute SQL on the GitBucket database.

## How to upgrade GitBucket?

See the [Installation](https://github.com/gitbucket/gitbucket#installation) section of README. 

We strongly recommend to backup your data before upgrade GitBucket.

## How to backup GitBucket data?

Basically, you can backup GitBucket data by copying `GITBUCKET_HOME` directory (`~/.gitbucket` in default) to the other place **after stopping GitBucket**.

See [[Backup]] also to check advanced topics about backup.

## How to contribute to gitbucket

You can help the project in many different ways:

- we need readers to comment/correct/translate in good english the existing documentation/FAQ/wiki
- we need help to write more documentation either in gitbucket main documentation, wiki or FAQ.
- we need help to consume/answer issues. If you know any answer to some issues, please help us answering them.
- when contributing, please follow our [contributing](https://github.com/gitbucket/gitbucket/blob/master/.github/CONTRIBUTING.md) guidelines and respect the templates when creating issues and pull-request