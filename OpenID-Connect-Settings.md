## Google Identity Provider tutorial:

Setup Google Identity Provider:

1. Open https://console.developers.google.com/apis/credentials and create an OAuth client ID.
1. Application type: web application
1. Authorized redirect URIs: `http://localhost:8080/signin/oidc`
1. Check the client ID and client secret of the created client.

Setup GitBucket:

1. Open the system settings and turn on OpenID Connect.
1. Issuer: `https://accounts.google.com`
1. Client ID: See Google Identity Provider
1. Client secret: See Google Identity Provider
1. Expected signature algorithm: RS256

Note that any Google user can sign in to your GitBucket and make sure restricted people can access to your GitBucket.

See also https://developers.google.com/identity/protocols/OpenIDConnect.

## Keycloak tutorial:

Setup Keycloak:

1. Create a new Client on your Keycloak.
1. Client ID: `gitbucket`
1. Client Protocol: openid-connect
1. Access Type: confidential
1. Valid Redirect URIs: `http://localhost:8080/signin/oidc`

Setup GitBucket:

1. Open the system settings and turn on OpenID Connect.
1. Issuer: `https://keycloak.example.com/auth/realms/YOUR_REALM` (replace `keycloak.example.com` with your host name and `YOUR_REALM` with your realm)
1. Client ID: `gitbucket`
1. Client secret: See Credetials tab in Keycloak
1. Expected signature algorithm: RS256
