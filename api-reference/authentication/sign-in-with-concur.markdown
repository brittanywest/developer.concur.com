# Sign in with Concur

## Why Concur Login?

Streamline user onboarding with Sign in with Concur – a new feature that allows new users to log on to a partner application using their Concur credentials. Similar to the single sign-on feature provided by Facebook and other social applications, Sign in with Concur reduces the time and effort involved in setting up an account with our partner apps. 

Concur users can simply login with their Concur credentials, leverage your application and then enjoy receiving itineraries, receipts and much more directly to their Concur account.

Our sign in process uses the secure OAuth2.0 protocol.

### Benefits

For partners

* Streamlines account set-up: User’s profile is pre-validated, including basic PII information plus travel preferences
*	Simplifies development and integration: Quickly obtain authentication tokens for the user and call Concur’s APIs

For users

* Quick and hassle-free way to sign up for a new app
* Secure: User’s Concur credentials are never shared with partners   

## Who can use this service?

This service is for TripLink Travel Suppliers and Third-party Developers interested in improving their sign in experience.

For TripLink Travel Suppliers, please see the appendix for information to support the available TripLink configurations.

## Certification

All Concur integrations are certified by our Partner Enablement team. Please use the below guide to get started. Then use our intake form to begin the certification process.

## Using OAuth2.0 and Concur's APIs

The below steps cover implementing "Sign in with Concur" within your website.

### Steps


#### 1. Request an app.
Before you can integrate Sign in with Concur into your application, you need to register your application with Concur. You can do this by contacting your Partner Enablement Manager or Partner Account Manager. Once you have registered an application, you will receive a clientId and clientSecret. The clientId is a unique UUID4 identifier for your application, and the clientSecret is your application password. You will be using this credential to obtain tokens either for the application itself, or on behalf of a user.


#### 2. Load the Concur style library.

```
<script src="https://static.concursolutions.com/" async defer></script>
```

#### 3. Add the "Sign in with Concur button". Styles will be applied from our style library.

```
<div class="concur-signin" data-onsuccess="onSignIn"></div>
```

#### 4. Initiate the Login flow. Your app must initiate the login flow by redirected to the below endpoint.

`GET /oauth2/v0/authorize`

Example:

```
https://www-us.api.concursolutions.com/oauth2/v0/authorize?client_id=<UUID> \
&redirect_uri=https%3A//www.hipmunk.com/auth/concur/callback \
&response_type=code \
&state=<state information>
```

This endpoint accepts the following parameters:

Name | Type | Format | Description
-----|------|--------|------------
client_id|string|UUID|Application client_id supplied by App Management
locale|string||Language, default to en_us, [see supported languages]
redirect_uri|string|UUID|URL to redirect to after the OAuth2 process
response_type|string||token or code, to select implicit and authorization grant. Implicit grant is recommended for user-agent-based clients
response_mode|string||query or fragment, default mode is assumed based on response_type (optional)
scope|string||One or more short scopes seprated by "", scopes can be found [here](https://developer.concur.com/api-reference/authentication/scopes.html)
product|string||com.concur.cte (default), com.concur.tripit (optional)
state|string||csrf token, arbitrary string provided by your app to prevent [CSRF attacks](https://en.wikipedia.org/wiki/Cross-site_request_forgery) (recommended)
nonce|string||User provided nonce string that has to be signed and sent back in the jwt (optional)

This renders the Sign in with Concur screen which presents two options to the user for signing in: 1) using Concur credentials OR 2) using a link sent via email. Option 2 is designed for users who do not want to use passwords or those that do not have passwords such as Single Sign On (SSO) users.

To view the Sign in with Concur flow from an end-user perspective, please see our user guide here.

#### 5. Once signed in, the user will be redirected to your redirect URI with a temporary token appended. 

```
Your_Redirect_Uri?
cc={token}
```

#### 6. Exchanging the token. Your app must parse the token and [exchange the token for a long lived refresh token](https://developer.concur.com/api-reference/authentication/apidoc.html#password_grant).

When the partner receives the redirect_uri call with the `cc` token, partner needs to call oauth2’s `/verify_otl` endpoint.

https://www-us.api.concursolutions.com/oauth2/v0/verify_otl?cc=<token>

The response for a successful call will look like this:

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Date: date-requested
Content-Length: 3397
Connection: Close
{
  "expires_in": "3600",
  "scope": "app-scope",
  "token_type": "Bearer",
  "access_token": "new-access_token",
  "refresh_token": "new-refresh_token",
  "refresh_expiry": "refresh_token_expiry",
  "geolocation": "https://us.api.concursolutions.com"
}
```

Tokens should be stored and an account created.

# Error Handling & Cancelled Sign In
In the case the user leaves the sign in process or sign in is unsuccessful, the user will be redirected to the following:

```
Your_Redirect_Uri?
 error_code=user_denied
 &error_description=The+user+denied+your+request.
```

Apps should then provide the user with alternative connection methods:
* Log in by creating an account
* Attempt Sign in with Concur with an alternate credential

# Account Context

Once connected, your app can access context to easily create a user account. Please refer to the [user API documentation](https://developer.concur.com/api-reference/user/) for more information.

# Managing Connections

Following Sign In, applications must:

* Maintain the tokens for future use.  To maintain the connection, please see the [token refresh documentation] (https://developer.concur.com/api-reference/authentication/apidoc.html#refresh_token).
* [Provide the option to disconnect](https://developer.concur.com/api-reference/authentication/apidoc.html#revoke_token)


# Advanced

## Merging Accounts
For partners that also support an existing log in, there are several instances that may require merging of accounts:

1. A customer has an existing account but would like to use Sign in with Concur.
2. A user previously using Sign in with Concur would like to create an application account.

In both cases, multiple accounts will exist. It is recommended that your app attempt to search for existing accounts and merge. The most common method is by leveraging email address from the [user API](https://developer.concur.com/api-reference/user/) or the loyalty program number from the [Travel Profile API](https://developer.concur.com/api-reference/travel-profile/03-loyalty-program-resource.html)
and matching existing accounts.

In some cases, users may leverage different email addresses within your application or the loyalty number may not be available and the user cannot be matched to data within Concur. Given that, we recommend that your app provide a separate "merge accounts" option allowing a user to manually merge their Concur and application accounts.

[TBP - which email addresses to use]

## TripLink Configurations

Depending on the products the customer has enabled, integrations and features available with Sign in with Concur vary. The following defines the scopes that are applicable product combinations. Your application must support each of the below potential scope configurations:

TripLink Access:
* Travel Profile
* Client Discount Code
* User form of Payment
* Ghost Card
* Itinerary
* E-Receipt

Non-Triplink Access:
* Travel Profile
* E-receipt

Expense Only Access:
* E-receipt

[TBD how TripLink apps will be notified of the applicable scopes - potentially modify scopes on the token exchange]

## Supported Languages

The following language codes are supported in by Sign in with Concur.

Code|Name
----|----
en| English (US)
en-gb| English (UK)
cs| Czech
da| Danish
de| German
fr| French
fr-ca| French (Canada)
es| Spanish
es-la| Spanish (Latin America)
el| Greek
it| Italian
lv| Latvian
lt| Lithuanian
hu| Hungarian
nl| Dutch (Netherlands)
pl| Polish
pt-br| Portugese
fi| Finnish
ru| Russian
sv| Swedish
sk| Slovak
tr| Turkish
ja| Japanese
zh-tw| Chinese (Traditional)
zh-cn| Chinese (Simplified)
ko| Korean


 
