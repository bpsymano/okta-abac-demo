# What?
This is a simple MuleSoft application used as part of a demonstration on attribute based access control (ABAC). The full context of this effort, including Okta and CloudHub configuration, is part of a blog on the subject [link TBD].

# Why?
The only intent of this app is to return a pre-canned response if and only if the incoming token passes JWT claims inpsection. The claims must included an entry for the requested resource identifier.
* __get:\contracts\(contractId):okta-abac-demo-config__

Please review the blog at [link TBD] as it includes the Okta, CloudHub Client Provider, and API Manager policy configurations.

# Testing

### Local
To test drive the app locally Add the following to your runtime config to bypass the API Manager:
* __-Danypoint.platform.gatekeeper=disabled__

### Cloudhub
Be sure to add your CloudHub properties for your environment
* __anypoint.platform.client_id__
* __anypoint.platform.client_secret__

Additionaly, you can add the following to the logging properties with __DEBUG__ level information:
* __org.mule.service.http.impl.service.HttpMessageLogger__
* __com.mulesoft.extension.policies.jwt__
* __com.myshittake.okta.abac__

### Postman configuration
Import and edit the Postman environment and collection files.

Update the environment file:
* __url__: your root URL created during the Okta sign-up e.g. dev-5652788.okta.com
* __apikey__: from the demo blog, this is your "cloudhub" token, giving you access to invoke the Okta APIs
* __sessionToken__: leave blank as it will auto populate from Postman script
* __accessToken__: leave blank, not used the the current collection
* __contracts_api_client_id__: this is the client_id from the Contracts API application createed in Okta (the app granted access to the demo API)
* __contracts_api_client_secret__: this is the client_secret from the Contracts API application createed in Okta

#### Grabbing a users access_token
##### Make an AuthN request

* The postman collection contains 3 sample requests, edit with impunity to meet you needs. Each request has a script that captures the *sessionToken* for use in the subsequant *Authorize* request.
* Okta doc: https://developer.okta.com/docs/reference/api/authn/

##### Make an Authorize request
* This Postman endpoint uses the captured *sessionToken* to begin the Oauth dance and callback to Postman. The *access_token* will be visible in the Postman console log to copy and use in the ABAC protected endpoint request.
* Okta doc: https://developer.okta.com/docs/guides/implement-oauth-for-okta/request-access-token/
