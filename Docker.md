Docker
======

To **quickly** run with Docker, just use:
```bash
% docker run -d -p 8080:8080 -p 29418:29418 -v ${PWD}/gitbucket-data:/gitbucket f99aq8ove/gitbucket
```
 - the UI will be reachable under `http://localhost:8080/` 
 - the GitBucket Data (config, DB, Git Repos, Plug-ins) will be persisted to `~/gitbucket-data`. 
 - to access this the git repository over SSH (port `29418`) too, one needs to enable it in the UI too: `GitBucket > Administration > System Settings > SSH access`

Images
------
There are several existing Docker images for GitBucket:

 - https://github.com/f99aq8ove/docker-gitbucket - oldest one (and mostly used) available around
 - https://github.com/takezoe/gitbucket-docker - directly from the author of GitBucket
 - https://hub.docker.com/r/million12/gitbucket/ - with built in nginx
 - many many more: https://hub.docker.com/search/?isAutomated=0&isOfficial=0&page=1&pullCount=0&q=gitbucket&starCount=0

Notes
-----
Thigs to consider when creating/using a Docker image:
 - the `GITBUCKET_HOME` is usually mapped outside the Docker container (to be able to update without loosing data)
 - one might want to put a reverse proxy in front of GitBucket, such as [Apache Httpd](https://github.com/gitbucket/gitbucket/wiki/Reverse-proxy-with-Apache), [NginX](https://github.com/gitbucket/gitbucket/wiki/Reverse-proxy-with-Nginx) or [Caddy](https://caddyserver.com/)
 - map the SSH port too if this is a desired feature (but also activate it from the UI)
 - don't forget about GitBucket plug-ins: they're mapped outside the container too, so when updating the Docker container they need quite often to be updated too.
 - if an [external DB](https://github.com/gitbucket/gitbucket/wiki/External-database-configuration) is desired (so not the built in H2), than `docker-compose` might be easier to manage: https://gitbucket.pgollor.de/docker/gitbucket