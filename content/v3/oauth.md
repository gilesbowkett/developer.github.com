---
title: OAuth | developer.github.com
---

# OAuth

OAuth2 is a protocol that lets external apps request authorization to
private details in a user's GitHub account without getting their
password. This is preferred over Basic Authentication because tokens can
be limited to specific types of data, and can be revoked by users at any
time.

All developers need to [register their
application](https://github.com/account/applications/new) before getting
started. A registered OAuth application is assigned a unique Client ID
and Client Secret. The Client Secret should not be shared.

## Web Application Flow

This is a description of the OAuth flow from 3rd party web sites.

### 1. Redirect users to request GitHub access

    GET https://github.com/login/oauth/authorize

### Parameters

client\_id
: _Required_ **string** - The client ID you received from GitHub when
you [registered](https://github.com/account/applications/new).

redirect\_uri
: _Optional_ **string** - URL in your app where user's will be sent
after authorization. See details below about [redirect
urls](#redirect-urls).

scope
: _Optional_ **string** - Comma separated list of [scopes](#scopes).

### 2. GitHub redirects back to your site

If the user accepts your request, GitHub redirects back to your site
with a temporary code in a code parameter. Exchange this for an access
token:

    POST https://github.com/login/oauth/access_token

### Parameters

client\_id
: _Required_ **string** - The client ID you received from GitHub when
you [registered](https://github.com/account/applications/new).

redirect\_uri
: _Optional_ **string**

client\_secret
: _Required_ **string** - The client secret you received from GitHub
when you [registered](https://github.com/account/applications/new).

code
: _Required_ **string** - The code you received as a response to [Step 1](#redirect-users-to-request-github-access).

### Response

access\_token
: _Required_ **string** - OAuth access token.

### 3. Use the access token to access the API

The access token allows you to make requests to the API on a behalf of a user.

    GET https://api.github.com/user?access_token=...

## Desktop Application Flow

Use basic authentication for now...

## Redirect URLs

The `redirect_uri` parameter is optional. If left out, GitHub will
redirect users to the callback URL configured in the OAuth Application
settings. If provided, the redirect URL must match the callback URL's
host.

    CALLBACK: http://foo.com

    GOOD: https://foo.com
    GOOD: http://foo.com/bar
    BAD:  http://foo:com:8080
    BAD:  http://bar.com

## Scopes

Scopes let you specify exactly what type of access you need. This will
be displayed to the user on the authorize form.

(no scope)
: public read-only access (includes public user profile info, public
repo info, and gists).

user
: DB read/write access to profile info only.

public\_repo
: DB read/write access, and Git read access to public repos.

repo
: DB read/write access, and Git read access to public and private repos.

gist
: write access to gists.

NOTE: Your application can request the scopes in the initial redirection. You
can specify multiple scopes by separating them by a comma.

    https://github.com/login/oauth/authorize?
      client_id=...&
      scope=user,public_repo


## More Information

It can be a little tricky to get started with OAuth. Here are a few
links that might be of help:

* [OAuth 2 spec](http://tools.ietf.org/html/draft-ietf-oauth-v2-07)
* [Facebook API](http://developers.facebook.com/docs/authentication/)
* [Ruby OAuth2 lib](https://github.com/intridea/oauth2)
* [simple ruby/sinatra example](https://gist.github.com/9fd1a6199da0465ec87c)
* [simple python example](https://gist.github.com/e3fbd47fbb7ee3c626bb) using [python-oauth2](http://github.com/dgouldin/python-oauth2)
* [Ruby OmniAuth example](http://github.com/intridea/omniauth)
* [Ruby Sinatra extension](http://github.com/atmos/sinatra_auth_github)
* [Ruby Warden strategy](http://github.com/atmos/warden-github)
* [Node.js demo using Nozzle](http://github.com/fictorial/nozzle/blob/master/demo/08-github-oauth2.js)
