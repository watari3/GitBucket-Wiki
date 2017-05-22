Performance
-----------

Please keep in mind that the **main** use case of GitBucket is for **small teams and projects**! 

Since the installation process is **trivial** however, one could: 
 - always host more than one GitBucket instance 
 - move problematic repos in their own instance
 - use external issue systems and Wikis (GitBucket allows linking them)

Other Tips
----
 - for (big) binary files in repos, activate the Git-LFS functionality: http://gitbucket.github.io/gitbucket-news/gitbucket/2017/01/03/git-lfs-support.html
 - tune https://github.com/gitbucket/gitbucket/wiki/Tracing-%26-logging **with care**
 - if the embedded H2 database seems to be the bottleneck, fine-tune the connection pool in ```$GITBUKET_HOME/database.conf```
 - external databases: MySQL and PostgreSQL are **not** faster than embedded H2: http://h2database.com/html/performance.html
 - please note that using DB Encryption https://github.com/gitbucket/gitbucket/wiki/External-database-configuration#database-encryption adds however a little extra overhead.

Benchmarks
----------

- https://gitbucket.github.io/gitbucket-news/gitbucket/2017/03/29/benchmark-of-gitbucket.html
