Authentication tokens are critical to secure access to Rocket Lawyer’s API services. This page provides a comprehensive overview of the three main types of tokens—Access Token, Service Token, and Scoped Access Token—along with guidance on using these tokens effectively during the document interview process.

Explore the different authentication tokens and their usage within Rocket Lawyer's API ecosystem through the sections below:

- [Access Token](#access-token)
- [Service Token](#service-token)
- [Scoped Access Token](#scoped-access-token)
- [Using Authentication Tokens During the Interview Process](#using-authentication-tokens-during-the-interview-process)

## Access Token

Access Tokens are used whenever an application needs to interact with Rocket Lawyer’s API services at a broad level. This includes starting new interviews, accessing all documents created by the app, or performing administrative tasks that involve multiple users.

To use an Access Token, generate it by calling the Authentication API with your app’s client key & secret, which you can obtain through [the RocketLawyer portal](https://developer.rocketlawyer.com/). Once received, include the Access Token in the Authorization header of your API requests to ensure secure communication with Rocket Lawyer’s services.

## Service Token

A Service Token performs specific purposes, such as generating the Scoped Access Token. It is generated with parameters like the purpose, interview ID, and Unique Party Identifier (UPID), ensuring it will only be used for its intended purpose.

Generate a Service Token by sending a request to the authentication endpoint with the necessary parameters. Once generated, use the Service Token to create a Scoped Access Token, which enforces restricted access to designated resources.

## Scoped Access Token

A Scoped Access Token is a secure token designed for frontend interactions where security is paramount. It grants restricted access to specific resources, such as documents or interviews linked to a particular user (identified by a UPID). This token is created using a Service Token, ensuring access is managed carefully and limited to the intended user.

To obtain a Scoped Access Token, you need to generate a Service Token first. Then, use the Service Token to request the Scoped Access Token from the authentication API. You should include this token in the Authorization header of your frontend API requests to maintain secure, user-specific access to the necessary resources.

## Using Authentication Tokens During the Interview Process

Authentication tokens are integral to managing the interview process securely and efficiently. Here’s how to use them:

### Creating an Access Token

Begin by generating an Access Token to authenticate the initial API requests. This token is used to start a new interview and retrieve the necessary document templates, establishing a secure foundation for the session. To generate the Access Token, you will send a request such as the one in the example below:

**Request:**
```curl
curl --request POST \
     --location 'https://api-sandbox.rocketlawyer.com/partners/v1/auth/accesstoken' \
     --header 'Content-Type: application/json' \
     --data '
{
    "grant_type": "client_credentials",
    "client_id": "{{partnerClientId}}",
    "client_secret": "{{partnerClientSecret}}"
}
'
```

In this request, you will send the `client_id` and `client_secret` values, which you can obtain through [the RocketLawyer portal](https://developer.rocketlawyer.com/). After logging in with your credentials, click on your email address in the top right corner and access the **Apps** dashboard. Select your app, go to the **API Keys** section, and find the `cliend_id` under **Key** and the `client_secret` under **Secret**.

**Response:**
```json
{
    "refresh_token_expires_in": "0",
    "api_product_list": "[partner-event-api-subscription-product-sandbox, rocketdoc-api-product-sandbox, partner-auth-service-product-sandbox, binders-product-document-manager-sandbox]",
    "api_product_list_json": [
        "partner-event-api-subscription-product-sandbox",
        "rocketdoc-api-product-sandbox",
        "partner-auth-service-product-sandbox",
        "binders-product-document-manager-sandbox"
    ],
    "organization_name": "rocketlawyer",
    "developer.email": "developer@rocketlawyer.com",
    "token_type": "BearerToken",
    "issued_at": "1723146278357",
    "client_id": "ZEAURAIq2q7ngQMBxRdvGFOQmy7q57xWxVW2nVP4EGzLy68H",
    "access_token": "{{generalAccessToken}}",
    "application_name": "eebb78c0-ab4f-4212-8665-b1292330dbf5",
    "scope": "",
    "expires_in": "35999",
    "refresh_count": "0",
    "status": "approved"
}
```

Your Access Token will returned in the `access_token` object. You should securely store this token in your backend for use in future requests.

### Creating a Service Token

Once the interview is initiated, create a Service Token to secure the session further. This token is then used to generate a Scoped Access Token, ensuring that access to the interview and associated documents is limited to the specific user. Below, you will see an example request for creating a Service Token:

**Request:**

```curl
curl --request POST \
     --location 'https://api-sandbox.rocketlawyer.com/partners/v1/auth/servicetoken' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {{generalAccessToken}}' \
     --data '
{
   "purpose" : "api.rocketlawyer.com/rocketdoc",
   "interviewId" : "42313c4d-11bb-414a-81a4-9fda067d1ae7",
   "partnerEndUserId" : "UNIVERSAL PARTY IDENTIFIER - UNIQUE ID TO REPRESENT A CUSTOMER",
   "expirationTime": 1753696695
}
'
```

This request will return a response with the service token string for the `token` object.

**Response:**

```json
{
    "purpose": "api.rocketlawyer.com/rocketdoc",
    "expirationTime": "1753696695",
    "token": "{{serviceToken}}"
}
```

### Creating a Scoped Access Token

As the interview progresses, use the Scoped Access Token to submit requests for each Interview Page. This token is used by the front-end application, which means it is visible to the browser. That is why you will not use the general Access Token, which allows access to all functionalities enabled for the partner. The Scoped Access Token allows access only to the informed `interviewId`. Create a Scoped Access Token using the Service Token as an authenticator for the request, as exemplified below:

**Request:**
```curl
curl --request POST \
     --location 'https://api-sandbox.rocketlawyer.com/partners/v1/auth/accesstoken' \
     --header 'Content-Type: application/json' \
     --data '
{
    "grant_type": "authorization_code",
	"client_id" : "{{partnerClientId}}",
	"client_secret" : "{{partnerClientSecret}}",
	"code" : "{{serviceToken}}"
}
'
```

The response will return the Scoped Access Token under the `access_token` object.

**Response:**
```json
{
    "refresh_token_expires_in": "0",
    "api_product_list": "[partner-event-api-subscription-product-sandbox, rocketdoc-api-product-sandbox, partner-auth-service-product-sandbox, binders-product-document-manager-sandbox]",
    "api_product_list_json": [
        "partner-event-api-subscription-product-sandbox",
        "rocketdoc-api-product-sandbox",
        "partner-auth-service-product-sandbox",
        "binders-product-document-manager-sandbox"
    ],
    "organization_name": "rocketlawyer",
    "developer.email": "developer@rocketlawyer.com",
    "token_type": "BearerToken",
    "issued_at": "1723146287236",
    "client_id": "{{partnerClientId}}",
    "access_token": "{{scopedAccessToken}}",
    "application_name": "eebb78c0-ab4f-4212-8665-b1292330dbf5",
    "scope": "",
    "expires_in": "35999",
    "refresh_count": "0",
    "status": "approved"
}
```
