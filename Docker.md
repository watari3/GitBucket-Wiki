Docker
======

To **quickly** run with Docker, just use:
```bash
% docker run -d -p 8080:8080 -p 29418:29418 -v ${PWD}/gitbucket-data:/gitbucket f99aq8ove/gitbucket
```
 - the Web UI will be reachable under `http://localhost:8080/` 
 - the `GitBucket` Data (config, DB, Git repos, plug-ins) will be persisted to `~/gitbucket-data`. 
 - to access this Git repository over SSH (port `29418`) too, one needs to enable it in the Web UI too: `GitBucket > Administration > System Settings > SSH access`

Images
------
There are several existing Docker images for `GitBucket`:

 - https://github.com/f99aq8ove/docker-gitbucket - oldest one (and mostly used) available around
 - https://github.com/takezoe/gitbucket-docker - directly from the author of GitBucket
 - https://hub.docker.com/r/million12/gitbucket/ - with built in nginx
 - https://gitbucket.pgollor.de/docker/gitbucket - for using with `docker-compose`
 - **many** more: https://hub.docker.com/search/?isAutomated=0&isOfficial=0&page=1&pullCount=0&q=gitbucket&starCount=0

Notes
-----
Thigs to consider when creating/using a Docker image:
 - the `GITBUCKET_HOME` is usually mapped outside the Docker container (to be able to update without loosing data)
 - one might want to put a reverse proxy in front of GitBucket, such as [Apache Httpd](https://github.com/gitbucket/gitbucket/wiki/Reverse-proxy-with-Apache), [NginX](https://github.com/gitbucket/gitbucket/wiki/Reverse-proxy-with-Nginx) or [Caddy](https://caddyserver.com/)
 - one can map the SSH port too, if this is desired (needs to be activated from the UI too)
 - don't forget about [GitBucket plug-ins](http://gitbucket-plugins.github.io/): 
   - they're mapped outside the container too, 
   - so when updating the Docker container they need quite often to be updated too. 
   - download the plug-ins working with latest GitBucket from: https://github.com/gitbucket-plugins/gitbucket-community.herokuapp.com/tree/master/plugins
 - if an [external DB](https://github.com/gitbucket/gitbucket/wiki/External-database-configuration) is desired (so not the built in H2), than [`docker-compose`](https://docs.docker.com/compose/overview/) might help. 
 - or use a UI like [Portainer](https://github.com/portainer/portainer) in order to manage your Docker.