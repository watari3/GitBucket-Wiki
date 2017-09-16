### Prerequisites
1. Is installed and running on your server.
2. GitBucket is running on some `port`. The default is `8080`.

### Apache HTTP server set up
1. Install and enable `mod_proxy` and `mod_proxy_http` modules.
2. Add the following to your site configuration (assuming your GitBucket prefix is "gitbucket"):
                
        AllowEncodedSlashes NoDecode # ... or "On" for Apache-2.2
        ProxyPreserveHost On
        ProxyPass /gitbucket http://localhost:8080/gitbucket
        ProxyPassReverse /gitbucket http://localhost:8080/gitbucket

> Note! Apache and GitBucket context paths must be equal:
`java -jar gitbucket.war --port=8080 --prefix=/gitbucket`
(Note the leading / in the prefix)