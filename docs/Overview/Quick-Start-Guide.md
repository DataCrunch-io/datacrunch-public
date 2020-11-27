---
tags: [Quick Start]
---

# Quick Start Guide

### 1. Get Credentials from the DataCrunch.io Dashboard

Go to the **Account Info** page and click on **Generate Credentials**:
![generate](../../assets/images/Credentials.png)

Give a name to your credentials and generate:
![generate](../../assets/images/Generated.png)

These credentials will be used like a username & password for your application. 

Store the client secret safely - like a password.


### 2. Generate Access Token

An Access Token is needed to be sent with any API call for authentication.

The Access Token has a limited lifetime and is expired after a while as defined in `expires_in`.

<br/>

#### To get an access token, call the token endpoint with the credentials you got on step 1:

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
    "client_secret": "XXXXXXXXXXXXXX"
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


When an Access Token expires, create a new one using the refresh token:

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

### 3. Make REST API Calls

That's it, you are ready to use the API, don't forget to add the Access Token to your calls:

Let's call the `/balance` endpoint. 

Add the Access Token value to the `Authorization` header, preceded by the "Bearer" string and a space:

```json http
{
  "method": "get",
  "url": "https://api.datacrunch.io/v1/balance",
  "headers": {
    "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJoZXkiOiJ5b3UgYWN1YWxseSBjaGVja2VkIHRoaXM_In0.0RjcdKQ1NJP9gbRyXITE6LFFLwKGzeeshuubnkkfkb8"
  }
}
```
