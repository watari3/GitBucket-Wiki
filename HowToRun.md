for Testers
--------

If you want to test GitBucket, input following command at the root directory of the source tree.

```
C:\gitbucket> sbt ~container:start
```

Then access to http://localhost:8080/ by your browser.

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