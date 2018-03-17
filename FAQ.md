FAQ
===

  1. [Can I test GitBucket actually before installation?](#1-can-i-test-gitbucket-actually-before-installation)
  1. [What's default root username and password?](#2-whats-default-root-username-and-password)
  1  [What's limitation of password?](#3-whats-limitation-of-password)
  1. [Docker image?](#4-docker-image)
  1. ["Unsupported major.minor version 52.0" Error?](#5-unsupported-majorminor-version-520-error)
  1. [```gitbucket-gist-plugin``` doesn't work after upgrading to GitBucket 4.x](#6-gitbucket-gist-plugin-doesnt-work-after-upgrading-to-gitbucket-4x)
  1. [How to upgrade GitBucket?](#7-how-to-upgrade-gitbucket)
  1. [How to backup GitBucket data?](#8-how-to-backup-gitbucket-data)
  1. [How to contribute to GitBucket?](#9-how-to-contribute-to-gitbucket)

-----
## 1. Can I test GitBucket actually before installation?

There are 2 online installations that demo GitBucket capabilities:

- _Official demo site_: https://gitbucket.herokuapp.com/  
This online demo site contains only the official GitBucket plugins. Login with `root` / `root`.
- _Community demo site_: https://gitbucket-community.herokuapp.com/  
This demo site contains mostly all known plugins ; i.e. the official ones + the ones delivered by the GitBucket community. Same login.

## 2. What's default root username and password?

root / root

## 3. What's limitation of password?

GitBucket has a limitation of password as below.

- Use alphanumeric character

If you use more strong password, GitBucket suggests to use external authorization system like LDAP.

## 4. Docker image?

See https://github.com/gitbucket/gitbucket/wiki/Docker for more details.

## 5. "Unsupported major.minor version 52.0" Error?

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

## 6. ```gitbucket-gist-plugin``` doesn't work after upgrading to GitBucket 4.x

If you had used [gitbucket-gist-plugin](https://github.com/gitbucket/gitbucket-gist-plugin) with GitBucket 3.x, it does not work after upgrading to GitBucket 4.x. Solution is below:

1. `UPDATE VERSIONS SET VERSION='2.0.0' WHERE MODULE_ID='gist';`
2. restart GitBucket
3. can open snippets page
4. `SELECT VERSION FROM VERSIONS WHERE MODULE_ID='gist'` -> `4.2.0`

See [[Connect to H2 database]] to know how to execute SQL on the GitBucket database.

## 7. How to upgrade GitBucket?

See the [Installation](https://github.com/gitbucket/gitbucket#installation) section of README. 

We strongly recommend to backup your data before upgrade GitBucket.

## 8. How to backup GitBucket data?

Basically, you can backup GitBucket data by copying `GITBUCKET_HOME` directory (`~/.gitbucket` in default) to the other place **after stopping GitBucket**.

See [[Backup]] also to check advanced topics about backup.

## 9. How to contribute to GitBucket?

You can help the project in many different ways:

- we need readers to comment/correct/translate in good English the existing documentation/FAQ/wiki
- we need help to write more documentation either in GitBucket main documentation, wiki or FAQ.
- we need help to consume/answer issues. If you know any answer to some issues, please help us answering them.
- when contributing, please follow our [contributing](https://github.com/gitbucket/gitbucket/blob/master/.github/CONTRIBUTING.md) guidelines and respect the templates when creating issues and pull-request