# Sign in with Concur

## Why Concur Login?

Sign in with Concur allows users to sign in to your application without creating a separate username and password. 

Concur users can simply login with their Concur credentials, leverage your application and then enjoy receiving itineraries, receipts and much more directly to their Concur account.

Our sign in process uses the secure OAuth2.0 protocol.


## Certification

All Concur integrations are certified by our Partner Enablement team. Please use the below guide to get started. Then use our intake form to begin the certification process.

## Using OAuth2.0 and Concur's APIs

The below steps cover implementing "Sign in with Concur" within your website.

### Steps

Before you begin, request an app here.

1. Load the Concur style library.

```
<script src="https://static.concursolutions.com/" async defer></script>
```

2. Add the "Sign in with Concur button". Styles will be applied from our style library.

```
<div class="concur-signin" data-onsuccess="onSignIn"></div>
```

3. Initiate the Login flow. Your app must initiate the login flow by redirected to the below endpoint.

```
https://www.concursolutions.com/oauth?
  client_id={app-id}
```
To view the Sign in with Concur flow from an end-user perspective, please see our user guide here.

4. Once signed in, the user will be redirected to your redirect URI with a temporary token appended. 

```
YourRedirectUri?requesttoken={token}
```
5. Exchanging the token. Your app must parse the token and [exchange the token for a long lived refresh token](https://developer.concur.com/api-reference/authentication/apidoc.html#password_grant).

Applications must:
* Maintain the tokens for future use.  To maintain the connection, please see the refresh documentation here: https://developer.concur.com/api-reference/authentication/apidoc.html#refresh_token
* [Provide the option to disconnect](https://developer.concur.com/api-reference/authentication/apidoc.html#revoke_token)

