FAQ
===

  1. [Can I test GitBucket actually before installation?](#q-1)
  1. What's default root username and password?
  1. Docker image?
  1. "Unsupported major.minor version 52.0" Error?
  1. ```gitbucket-gist-plugin``` doesn't work after upgrading to GitBucket 4.x
  1. How to upgrade GitBucket?
  1. How to backup GitBucket data?
  1. [How to contribute to GitBucket?](#q-8)

-----
## 1. Can I test GitBucket actually before installation?[](#q-1)

There are 2 online installations that demo GitBucket capabilities:

- _Official demo site_: https://gitbucket.herokuapp.com/  
This online demo site contains only the official GitBucket plugins. Login with `root` / `root`.
- _Community demo site_: https://gitbucket-community.herokuapp.com/  
This demo site contains mostly all known plugins ; i.e. the official ones + the ones delivered by the GitBucket community. Same login.

## [2. What's default root username and password?](#q-2)

root / root

## [3. Docker image?](#q-3)

If you have docker host, this command just works(See [docker-gitbucket](https://github.com/f99aq8ove/docker-gitbucket)):

`% docker run -d -p 8080:8080 -p 29418:29418 -v ${PWD}/gitbucket-data:/gitbucket f99aq8ove/gitbucket`

Then you can access via HTTP to http://localhost:8080/, and you can also [[Reverse-proxy-with-Nginx]].

Need plugins? Then you can allocate plugin JARs into `${PWD}/gitbucket-data/plugins/` (create if not exists), and restart the container.

## [4. "Unsupported major.minor version 52.0" Error?](#q-4)

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

## [5. ```gitbucket-gist-plugin``` doesn't work after upgrading to GitBucket 4.x](#q-5)

If you had used [gitbucket-gist-plugin](https://github.com/gitbucket/gitbucket-gist-plugin) with GitBucket 3.x, it does not work after upgrading to GitBucket 4.x. Solution is below:

1. `UPDATE VERSIONS SET VERSION='2.0.0' WHERE MODULE_ID='gist';`
2. restart GitBucket
3. can open snippets page
4. `SELECT VERSION FROM VERSIONS WHERE MODULE_ID='gist'` -> `4.2.0`

See [[Connect to H2 database]] to know how to execute SQL on the GitBucket database.

## [6. How to upgrade GitBucket?](#q-6)

See the [Installation](https://github.com/gitbucket/gitbucket#installation) section of README. 

We strongly recommend to backup your data before upgrade GitBucket.

## [7. How to backup GitBucket data?](#q-7)

Basically, you can backup GitBucket data by copying `GITBUCKET_HOME` directory (`~/.gitbucket` in default) to the other place **after stopping GitBucket**.

See [[Backup]] also to check advanced topics about backup.

## [](#q-8)8. How to contribute to GitBucket?

You can help the project in many different ways:

- we need readers to comment/correct/translate in good English the existing documentation/FAQ/wiki
- we need help to write more documentation either in GitBucket main documentation, wiki or FAQ.
- we need help to consume/answer issues. If you know any answer to some issues, please help us answering them.
- when contributing, please follow our [contributing](https://github.com/gitbucket/gitbucket/blob/master/.github/CONTRIBUTING.md) guidelines and respect the templates when creating issues and pull-request