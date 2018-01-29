GitBucket supports the OpenID Connect authentication since 4.21.0.

## Prerequisite

Check if your IdP satisfies the followings:

- A client ID and client secret must be given by the IdP.
- The IdP must provide an user identifier by the `sub` claim.
- The IdP must provide an email address by the `email` claim.
- The IdP should provide full name of a user by the `name` claim.
- The IdP may provide a username by the `preferred_username` claim.

GitBucket does the followings on the OpenID Connect authentication:

1. Try to extract the username for the identity from:
    - If the `preferred_username` claim is given, use it.
    - If the `email` claim is given, use first part of it (before `@`).
1. Check if the username already exists:
    - If the username already exists, raise an error.
    - If the username does not exist, create a user.
1. Federate the user and the identity (i.e. the pair of `iss` and `sub` claim).
1. Sign in as the user.

## Getting Started

### Google Identity Provider (Google Apps)

Setup Google Identity Provider:

1. Open https://console.developers.google.com/apis/credentials
1. Create an OAuth client ID.
    - Application type: web application
    - Authorized redirect URIs: `http://localhost:8080/signin/oidc`
1. Check the client ID and client secret of the OAuth client.

Setup GitBucket:

1. Sign in as an administrator.
1. Open the system settings.
1. Turn on OpenID Connect and enter the followings:
    - Issuer: `https://accounts.google.com`
    - Client ID: See Google Identity Provider
    - Client secret: See Google Identity Provider
    - Expected signature algorithm: RS256
1. Sign out.
1. Sign in with OpenID Connect.

Note that any Google users can sign in to your GitBucket. Make sure restricted people can access to your GitBucket.

See also https://developers.google.com/identity/protocols/OpenIDConnect.

### Keycloak

Setup Keycloak:

1. Create a new Client on your Keycloak.
    - Client ID: `gitbucket`
    - Client Protocol: openid-connect
    - Access Type: confidential
    - Valid Redirect URIs: `http://localhost:8080/signin/oidc`

Setup GitBucket:

1. Sign in as an administrator.
1. Open the system settings.
1. Turn on OpenID Connect and enter the followings:
    - Issuer: `https://keycloak.example.com/auth/realms/YOUR_REALM` (replace `keycloak.example.com` with your host name and `YOUR_REALM` with your realm)
    - Client ID: `gitbucket`
    - Client secret: See Credetials tab in Keycloak
    - Expected signature algorithm: RS256
1. Sign out.
1. Sign in with OpenID Connect.
