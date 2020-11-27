---
tags: [Authentication]
---

# Authentication

We use [OAuth 2.0 **Client Credentials**](https://oauth.net/2/grant-types/client-credentials/) authentication scheme.

Users are able to generate credentials via the dashboard UI - for external applications to use datacrunch's api. 

The credentials are used to generate Access & Refresh tokens. 

All requests to datacrunch's api must be authenticated using the Access token.

Access tokens have a short lifespan, and can be regenerated using the Refresh token.

Example of client credentials:
```
cliend id: Ibk5bdxV64lKAWOqYnvSi
client secret: Z4CZq02rdwdB7ISV0k4Z2gtwAFKiyvr2U1l0KDIeYi
```

Example of an Access / Refresh token:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJoZXkiOiJ5b3UgYWN1YWxseSBjaGVja2VkIHRoaXM_In0.0RjcdKQ1NJP9gbRyXITE6LFFLwKGzeeshuubnkkfkb8
```


## How to generate client credentials:
Go to the **Account Info** page and click on **Generate Credentials**:
![generate](../../assets/images/Credentials.png)

Give a name to your credentials and generate:
![generate](../../assets/images/Generated.png)

These credentials will be used like a username & password for your application. 

Store the client secret safely - like a password.

## How to generate Access & Refresh tokens:

To get an access token, call the token endpoint with the generated credentials:
```json http
{
  "method": "post",
  "url": "https://api.datacrunch.io/v1/oauth2/token",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "grant_type": "client_credentials",
    "client_id": "XXXXXXXXXXXXXX",
    "client_secret": "XXXXXXXXXXXXXXXXXXXXXXXXXXX"
  }
}
```

A valid response would look similar to this:

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJoZXkiOiJ5b3UgYWN1YWxseSBjaGVja2VkIHRoaXM_In0.0RjcdKQ1NJP9gbRyXITE6LFFLwKGzeeshuubnkkfkb8",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3ciOiJhbmQgdGhpcyB0b28_In0.AC5gk-o-MOptUgrouEErlhr8WT3Hg_RR6px6A0I7ZEk",
  "scope": "fullAccess"
}

```

`access_token` is the token value, to be used in the next step.

`expires_in` is the duration in seconds until the token is expired.

`refresh_token` can be used to create a new access token if expired.

`scope` is your app's permission scope. currently only `fullAccess`is enabled.

`token_type` means this is a Bearer token, to be added as a Bearer token in the Authentication header of the http request.


## How to add the access token to an API call:

Add the Access Token value to the `Authorization` header, preceded by the "Bearer" string and a space:

```json
{
  "method": "get",
  "url": "https://api.datacrunch.io/v1/balance",
  "headers": {
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJoZXkiOiJ5b3UgYWN1YWxseSBjaGVja2VkIHRoaXM_In0.0RjcdKQ1NJP9gbRyXITE6LFFLwKGzeeshuubnkkfkb8"
  }
}
```


## How to refresh an Access token using the refresh token:

When an Access Token expires, create a new one using the refresh token and calling the token endpoint with a `refresh_token` grant type:

```json http
{
  "method": "post",
  "url": "https://api.datacrunch.io/v1/oauth2/token",
  "headers": {
    "Content-Type": "application/json"
  },
  "body": {
    "grant_type": "refresh_token",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ3b3ciOiJhbmQgdGhpcyB0b28_In0.AC5gk-o-MOptUgrouEErlhr8WT3Hg_RR6px6A0I7ZEk"
    }
}
```

The response will include a new token.