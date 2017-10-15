Prerequisites
------------

1. [Caddy](https://caddyserver.com/) installed and running
2. GitBucket running on some `PORT` (the default is 8080). For easier config use also the prefix `--prefix=gitbucket`.
3. Know the `DOMANIN` you want GitBucket to be run on.

Configuration
-------------
Add the following to your Caddy config file:
```
DOMAIN {
    root /srv/site/www
    gzip
    log  /var/logs/sites/access.log
    proxy /gitbucket localhost:PORT {
        transparent
    }
}
```

Notes
-----

Besides the extremely simple configuration, the main advantage of Caddy is the the automatic SSL certificate handling https://caddyserver.com/features .

To run Caddy as a systemd service, follow https://github.com/mholt/caddy/tree/master/dist/init/linux-systemd