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

'''<script src="https://static.concursolutions.com/" async defer></script>
'''

2. Add the "Sign in with Concur button" and client ID to your page. Specify the app ID as:

'''<meta name="concur-signin-app_id" app_id="<app_id>">
'''

3. Add the "Sign in with Concur button". Styles will be applied from our style library.

'''<div class="concur-signin" data-onsuccess="onSignIn"></div>
'''

To view the Sign in with Concur flow from an end-user perspective, please see our user guide here.
