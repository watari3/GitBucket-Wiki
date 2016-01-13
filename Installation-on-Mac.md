## Installing Via Homebrew

```
$ brew install gitbucket
==> Downloading https://github.com/takezoe/gitbucket/releases/download/1.10/gitbucket.war
######################################################################## 100.0%
==> Caveats
Note: When using launchctl the port will be 8080.

To have launchd start gitbucket at login:
    ln -sfv /usr/local/opt/gitbucket/*.plist ~/Library/LaunchAgents
Then to load gitbucket now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.gitbucket.plist
Or, if you don't want/need launchctl, you can just run:
    java -jar /usr/local/opt/gitbucket/libexec/gitbucket.war
==> Summary
/usr/local/Cellar/gitbucket/1.10: 3 files, 42M, built in 11 seconds
```

## Manual Installation
On OS X, generate `gitbucket.plist` by [this script](https://raw.githubusercontent.com/gitbucket/gitbucket/master/contrib/macosx/makePlist) and copy it to `~/Library/LaunchAgents/`

Run the following commands in `Terminal` to

- start gitbucket: `launchctl load ~/Library/LaunchAgents/gitbucket.plist`
- stop gitbucket: `launchctl unload ~/Library/LaunchAgents/gitbucket.plist`