#OAuth
```always
  /$$$$$$   /$$$$$$              /$$     /$$      
 /$$__  $$ /$$__  $$            | $$    | $$      
| $$  \ $$| $$  \ $$ /$$   /$$ /$$$$$$  | $$$$$$$ 
| $$  | $$| $$$$$$$$| $$  | $$|_  $$_/  | $$__  $$
| $$  | $$| $$__  $$| $$  | $$  | $$    | $$  \ $$
| $$  | $$| $$  | $$| $$  | $$  | $$ /$$| $$  | $$
|  $$$$$$/| $$  | $$|  $$$$$$/  |  $$$$/| $$  | $$
 \______/ |__/  |__/ \______/    \___/  |__/  |__/                                                          
                             
```

Dwolla's API lets you interact with a user's Dwolla account and act on its behalf to send money, request money, and more.  To do so, your application first needs to request authorization from users.  

Dwolla implements the [OAuth 2.0 standard](http://oauth.net/2/) to facilitate this authorization. Similar to Facebook and Twitter's authentication flow, the user is first presented with a permission dialog for your application, at which point the user can either approve the permissions requested, or reject them. Once the user approves, an `authorization_code` is sent to your application, which will then [be exchanged](#finish-authorization) for an `access_token` and a `refresh_token` pair. 

The `access_token` can then be used to make API calls which require user authentication like [Send](#send-money) or [List Transactions](#list-a-user's-transactions). 

### Token lifetimes

**Access tokens** are *short lived*: 1 hour.

**Refresh tokens** are *long lived*: 60 days.

A refresh token can be used within 60 days to generate a new access_token and refresh_token pair.  So long as you [refresh your authorization](#refresh-authorization) at least every 60 days, your application can maintain authorization indefinitely without requiring the user to re-authorize.

## Request Authorization

```php
<?php

$OAuth = new Dwolla\OAuth();
$OAuth->settings->client_id = $apiKey;
$OAuth->settings->client_secret = $apiSecret;

$url = $OAuth->genAuthUrl("https://myredirect.com/redirect");

?>
```
```python
# Generate OAuth URL with redirect to requestb.in
print(oauth.genauthurl("http://requestb.in/122rdhc1"))
```
```js
// where to send the user after they grant permission:
var redirect_uri = "https://www.myredirect.com/redirect";  

// generate OAuth initiation URL
var authUrl = Dwolla.authUrl(redirect_uri);
```

```ruby
redirect_uri = "https://www.myredirect.com/redirect"
authUrl = Dwolla::OAuth.get_auth_url(redirect_uri)
```

> Example initiation URL:

```shell
https://www.dwolla.com/oauth/v2/authenticate?client_id=abcdefg&response_type=code&redirect_uri=https%3A%2F%2Fwww.myredirect.com%2Fredirect&scope=send|transactions
```

To start the OAuth process, construct the initiation URL which the user will visit in order to grant permission to your application.  It describes the permissions your application requires (`scope`), who the client application is (`client_id`), and where the user should be redirected to after they grant or deny permissions to your application (`redirect_uri`).


### URL Format:

`
https://www.dwolla.com/oauth/v2/authenticate?client_id={client_id}&response_type=code&redirect_uri={redirect_uri}&scope={scope}
`


Parameter | Optional |Description
----------|----------|------------
client_id | | Application key
response_type | | This must always be set to `code`
redirect_uri | | URL where the user will be redirected to afterwards. The value of this parameter must match one of the values that appear in your [application details](https://www.dwolla.com/applications) page. (We compare: protocol, subdomain, domain, tld, and file path. Querystring parameters are ignored)
scope | | Permissions you are requesting.  See [below](#oauth-scopes) for list of available scopes.  Scopes are delimited by a pipe ("&#124;")
verified_account | yes | Require new users opting to register for Dwolla to create a fully-verified Dwolla account instead of a default lightweight Direct account. `true` or `false`
dwolla_landing | yes | An optional override that force displays either the login or create an account screen. Valid values are: `login`, `register`, or `null`.

<aside class="notice">
Remember to url-encode all querystring parameters!
</aside>


### OAuth Scopes

Applications may request the following permission scopes when generating an access token:

Scope Name | Description
-----------|-------------
AccountInfoFull | Access detailed account profile
Contacts | Access the user's contacts
Transactions | Access the user's transaction data
Balance | Access the user's Dwolla account balance
Send | Send money on the user's behalf
Request | Send money requests, list them, fulfill them
Funding | Access names of funding sources the user has connected to Dwolla, access available balance information for Dwolla Balance and Dwolla Credit (if applicable), add new funding sources, verify funding sources, initiate transfers to and from funding sources
ManageAccount | Manage the user's account settings
Scheduled | Allow scheduling one-time and recurring payments
Email | Access the user's Dwolla account email address

## Finish Authorization

```php
<?php

$OAuth = new Dwolla\OAuth();
$OAuth->settings->client_id = $apiKey;
$OAuth->settings->client_secret = $apiSecret;

$authorizationCode = "J9kkk2JbX7Yjl4L28fM13il46QI=";
$redirect_uri = "https://www.myredirect.com/redirect";
$result = $OAuth->get($authorizationCode, $redirect_uri);

print_r($result);
?>
```
```python
# Get access key and refresh token pair
access_set = oauth.get("Z/KHDIyWO/LboIGn3wGGs1+sRWg=", "http://requestb.in/122rdhc1")
print(access_set)
```
```json
{
  "client_id": "JCGQXLrlfuOqdUYdTcLz3rBiCZQDRvdWIUPkw++GMuGhkem9Bo",
  "client_secret": "g7QLwvO37aN2HoKx1amekWi8a2g7AIuPbD5C/JSLqXIcDOxfTr",
  "code": "h6TvQZH+5BsV//O43uOJ0uRkBLk=",
  "grant_type": "authorization_code",
  "redirect_uri": "https://www.myredirect.com/redirect"
}
```

```js
Dwolla.finishAuth(authorizationCode, redirect_uri, function(error, auth) {
  var access_token = auth.access_token;
  var refresh_token = auth.refresh_token;
});
```

```ruby
info = Dwolla::OAuth.get_token(code, redirect_uri)
token = info['access_token']
refresh_token = info['refresh_token']
```

> Successful Response:

```shell
{
	"access_token": "YLAZSreh165CAD2tPhAEzFCYYIrVyFomLWUDMGFBZIw9KtIg4q",
	"expires_in": 3600,
	"refresh_token": "Pgk+l9okjwTCfsvIvEDPrsomE1er1txeyoaAkTIBAuXza8WvZY",
	"refresh_expires_in": 5184000,
	"token_type": "bearer"
}
```

Once the user returns to your application via the `redirect_uri` you specified, there will be a `code` querystring parameter appended to that URL.  Exchange the authorization `code` for an `access_token` and `refresh_token` pair.

### HTTP Request
`POST https://www.dwolla.com/oauth/v2/token`

Parameter | Description
----------|------------
client_id | Application key
client_secret | Application secret
code | The authorization code included in the redirect URL
grant_type | This must be set to `authorization_code`
redirect_uri | The same redirect_uri specified in the intiation step

### Response Parameters

Parameter | Description
----------|------------
access_token | A new access token with requested scopes
expires_in | The lifetime of the access token, in seconds.  Default is 3600.
refresh_token | New refresh token
refresh_expires_in | The lifetime of the refresh token, in seconds.  Default is 5184000.
token_type | Always `bearer`.

## Refresh Authorization
```json
{
  "client_id": "JCGQXLrlfuOqdUYdTcLz3rBiCZQDRvdWIUPkw++GMuGhkem9Bo",
  "client_secret": "g7QLwvO37aN2HoKx1amekWi8a2g7AIuPbD5C/JSLqXIcDOxfTr",
  "refresh_token": "Pgk+l9okjwTCfsvIvEDPrsomE1er1txeyoaAkTIBAuXza8WvZY",
  "grant_type": "refresh_token"
}
```
```python
# Exchange your expiring refresh token in "access_set" for another
# access/refresh token pair

print(oauth.refresh(access_set['refresh_token']))
```
```js
Dwolla.refreshAuth(refreshToken, function(error, auth) {
  var new_access_token = auth.access_token;
  var new_refresh_token = auth.refresh_token;
});
```

```ruby
info = Dwolla::OAuth.refresh_auth(refresh_token)
token = info['access_token']
refresh_token = info['refresh_token']
```

```php
<?php

$OAuth = new Dwolla\OAuth();
$OAuth->settings->client_id = $apiKey;
$OAuth->settings->client_secret = $apiSecret;

$refreshToken = "Cr72k48ogBXh+PLwZ/gq2hAtRYSTQl+NW9W0fTxYjaYKRdEsKI";

$result = $OAuth->refresh($refreshToken);
?>
```

> Successful Response:

```shell
{
	"access_token": "hr1tSJk23tOv9RxR8cQuDpUD/kpUgf0cb9WfKtRoPxTw8ymbVt",
	"expires_in": 3600,
	"refresh_token": "lqopeyjq5AJln5fpMedElUULz+XBNNw9nAkIinxE0g4aEzxmDc",
	"refresh_expires_in": 5184000,
	"token_type": "bearer"
}
```

> Invalid or Expired Refresh Token Response:

```shell
{
  "error": "access_denied",
  "error_description": "Invalid refresh token."
}
{
  "error": "access_denied",
  "error_description": "Expired refresh token."
}
```

Use a valid `refresh_token` to generate a new `access_token` and `refresh_token` pair.

**NOTE:** The `refresh_token` you receive will *change* every time you exchange either an `authorization_code` or `refresh_token` for a new token pair. Only the most recently issued `refresh_token` will allow you to receive a new pair.

### HTTP Request
`POST https://www.dwolla.com/oauth/v2/token`

Parameter | Description
----------|------------
client_id | Application key
client_secret | Application secret
refresh_token | A valid refresh token
grant_type | This must be set to `refresh_token`

### Response Parameters

Parameter | Description
----------|------------
access_token | A new access token with requested scopes
expires_in | The lifetime of the access token, in seconds.  Default is 3600.
refresh_token | New refresh token
refresh_expires_in | The lifetime of the refresh token, in seconds.  Default is 5184000.
token_type | Always `bearer`.
