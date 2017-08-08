### Prerequisites
1. Is installed and running on your server.
2. GitBucket is running on some `port`. The default is `8080`.
3. Know what domain you want GitBucket running

Nginx uses a configuration structure for virtual hosts that looks like */etc/nginx/sites-available* and */etc/nginx/sites-enabled*. This is fairly self-explanatory: the *sites-available* directory is site configuration files that are available and may more or may not be enabled and *sites-enabled* are site configuration files which are live and Nginx is serving.

### Configuration
1. Create a file like */etc/nginx/sites-available/gitbucket*
2. Add the following to the file:

```
server {
    listen   80; # The default is 80 but this here if you want to change it.
    server_name gitbucket.example.com;
    
    location / {
        proxy_pass              http://localhost:8080;
        proxy_set_header        Host $host;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_connect_timeout   150;
        proxy_send_timeout      100;
        proxy_read_timeout      100;
        proxy_buffers           4 32k;
        client_max_body_size    500m; # Big number is we can post big commits.
        client_body_buffer_size 128k;
    }
}
```

3. Make a symlink to *sites-enabled*. E.g.:

```
ln -s /etc/nginx/sites-available/gitbucket /etc/nginx/sites-enabled/
```

4. Check the Nginx configuration `nginx -t`
5. If that returns no errors, restart your Nginx server.

### Assets caching

To improve GitBucket performance, you should add the following configuration to enable assets caching:

```
server {
    ...
    location /assets/ {
        proxy_pass              http://localhost:8080/assets/;
        proxy_cache             cache;
        proxy_cache_key         $host$uri$is_args$args;
        proxy_cache_valid       200 301 302 1d;
        expires                 1d;
    }
}
```

If you have never configured the cache zone, add the following line to the top level of the Nginx configuration file.

```
proxy_cache_path /var/lib/nginx/cache levels=1:2 keys_zone=cache:512m inactive=1d  max_size=60g;
```
