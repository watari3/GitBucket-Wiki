for Testers
--------

If you want to test GitBucket, input following command at the root directory of the source tree.

```
C:\gitbucket> sbt ~container:start
```

Then access to **http://localhost:8080/** by your browser. The default administrator account is **root** and password is **root**.

for Developers
--------
If you want to modify source code and confirm it, you can run GitBucket in auto reloading mode as following:

```
C:\gitbucket> sbt
...
> container:start
...
> ~ ;copy-resources;aux-compile
```

Build war file
--------

To build war file, run the following command:

```
C:\gitbucket> sbt package
```

**gitbucket_2.10-x.x.x.war** is generated into **target/scala-2.10**.