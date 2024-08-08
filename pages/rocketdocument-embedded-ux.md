Welcome to the **RocketDocument™ Embedded UX** Quick Start guide. This section provides a step-by-step process for creating and customizing a Rocket Lawyer document using a simple interview-style experience. By following these steps, you'll be able to complete a document and display it using the **RocketSign® Embedded UX**. 

## Items Required

Ensure you have the following items ready to create and customize a Rocket Lawyer document:

1. **Client Credentials**   
     Obtain these during the onboarding process, detailed in the [Welcome Guide](welcome-guide).  

2. **templateId**   
     This is the unique ID for the template used to initialize the interview. For example, for a lease agreement, you can use: `04d9d0ba-3113-40d3-9a4e-e7b226a72154`.

3. **partnerEndUserId**     
     An ID that identifies the end user in your system.

4. **partyEmailAddress**        
     The email address of the end user for document notifications.

> **Note:** Use `api-sandbox.rocketlawyer.com` for testing environments. When you're ready for production, switch to `api.rocketlawyer.com`. This applies to all interactions with the RocketDocument API.     

## Getting Started

Follow the steps below to complete this guide:

1. [Generate an Access Token](#step-1-generate-an-access-token)
2. [Create the Interview](#step-2-create-the-interview)
3. [Access the RocketDocument UI](#step-3-access-the-rocketdocument-ui)
4. [Display Your Document](#step-4-display-your-document)
5. [Integrate with RocketSign (Optional)](#step-5-optional-integrate-with-rocketsign)

### Step 1: Generate an Access Token

Before interacting with the RocketDocument API, you must authenticate your calls by obtaining an Access Token. This token will be used to authorize all subsequent API requests. In this step, you will learn how to generate an Access Token by calling the Authentication API.

Authenticate each call to the **RocketDocument API** by obtaining an Access Token. Call the [Authentication API](/docs/partner-auth-service-product-sandbox/1/routes/accesstoken/post) as follows:

```http
POST https://api-sandbox.rocketlawyer.com/partners/v2/auth/accesstoken
```

Include the correct credentials (`client_id` and `client_secret`) and `grant_type`:

```json
{
  "client_id": "{api-key}",
  "client_secret": "{api-secret}",
  "grant_type": "client_credentials"
}
```

The response includes an Access Token:

```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.(content redacted).tBX73KTTjopBSRDL0cIBt3EK_DcA3Jc9KKonbpBn6HE"
}
```

> **Note:** Token content has been redacted for security reasons.

### Step 2: Create the Interview

With the Access Token in hand, the next step is to create an interview session for the document. This interview is based on a specific document template and will gather the necessary information to customize the document. This step will guide you through making a POST request to the RocketDocument API to create the interview.

**Assumptions**

- You have the `templateId` for the template to base the interview on. This guide uses a Lease Agreement `templateId`. For other document types, use the corresponding `templateId`.
- You have a valid `partnerEndUserId`.

**Request**

Create an interview by making a POST request to the [RocketDocument API](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/post):

```http
POST https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews
```

Include the Access Token in the Authorization header:

```http
Authorization: Bearer {accessToken}
```

Request body:

```json
{
  "templateId": "04d9d0ba-3113-40d3-9a4e-e7b226a72154",
  "partyEmailAddress": "me@emailaddress.com",
  "partnerEndUserId": "cfd1ee5a-061a-40cc-be72-8cbb9945b5d9"
}
```

**Response**

Response body:

```json
{
  "interviewId": "0af17ba7-f332-5346-bb3f-00b7c9af7deb"
}
```

Response header:

```http
rl-rdoc-servicetoken: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.(content redacted).zZOGi0TbfqzzCnJbo8hbpDnNwkjr4sPxeTi9KyIT3LY
```

Use the `rl-rdoc-servicetoken` for all subsequent interview-related calls. Include it in the Authorization header:

```http
Authorization: Bearer {rl-rdoc-servicetoken}
```

### Step 3: Access the RocketDocument UI

Now that the interview is set up, you need to embed the RocketDocument UX into your platform’s UI. This step involves adding specific HTML elements and JavaScript to load the interactive interview interface. Here, you will learn how to include the required script and web component tags in your HTML.

Add this script tag in the header:

```html
<script src="https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js"></script>
```

Insert this web component tag in the body:

```html
<rocket-document serviceToken="{rl-rdoc-servicetoken}" interviewId="{interview-id}"></rocket-document>
```

- `{rl-rdoc-servicetoken}`: The service token from Step 1. Learn more in the [Authentication API Documentation](/docs/partner-auth-service-product-sandbox/1/overview).
- `{interview-id}`: From the response in Step 2.

> **Attention!** `{interview-id}` is different from `templateId`.

Simplified webpage example:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>RocketDocument Demo</title>
    <script src="https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js"></script>
  </head>
  <body>
    <div class="content">
      <rocket-document 
        serviceToken="..." 
        interviewId="..."></rocket-document>
    </div>
  </body>
</html>
```

### Step 4: Display Your Document

After loading RocketDocument Embedded UX, your interview should be interactive. Make sure that the interview loads properly and offers a visual representation of the embedded interface, as shown in the image below. At this point, your document will be completely interactive.

![RocketDocument Embedded UX](https://rl-cicdv2-apigee-public-prod.apigee.io/files/RocketDocument-Embedded-Mobile.png)

> **Congratulations!** You've successfully displayed the document using **RocketDocument Embedded UX**.

### Step 5 (Optional): Integrate with **RocketSign**

For platforms that require digital signatures, integrating RocketSign with RocketDocument adds this capability. In this step, you will learn how to retrieve the interview JSON object, save the **binderId**, and reuse the service token for RocketSign. This optional step extends the functionality of your integrated solution to include electronic signatures.

To integrate with **RocketSign**, retrieve the Interview JSON Object and save the **binderId**. Keep the service token (`rl-rdoc-servicetoken`) for reuse with RocketSign.

**Request**

```http
GET https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{interviewId}
```

**Example Response**

```json
{
  "binder": {
    "binderId": "0af17ba7-f332-5346-bb3f-00b7c9af7deb",
    "documentId": "7d989647-ecf2-4673-9486-80c3b890ed3c"
  },
  "interview": {
    "answersPayload": {
      "version": 2,
      "Fk8jctrn744ku5": true,
      "Fk8jd1no93zprz": "My Business",
      "Fk8jd4pfntjpvf": false,
      "Fk8jdel8mfwiot": "415 9999999"
    },
    "createdAt": "2020-12-01T17:51:40.795Z",
    "updatedAt": "2021-12-01T18:51:40.795Z",
    "interviewName": "Employment contract",
    "interviewId": "0af17ba7-f332-5346-bb3f-00b7c9af7deb",
    "interviewStatus": "created",
    "templateVersionId": "7d989647-ecf2-4673-9486-80c3b890ed3c"
  },
  "template": {
    "name": "string",
    "payload": "string"
  }
}
```

## Next Steps

You’ve successfully created, displayed, and interacted with a document interview. To further enhance your integration and explore additional capabilities, check out the following resources:

**Quick Starts**
- [Quick Start: RocketSign Embedded UX](rocketsign-embedded-ux)

**API Documentation**
- [RocketDocument API Documentation](/docs/rocketdoc-api-product-sandbox/1/overview)
- [Authentication API Documentation](/docs/partner-auth-service-product-sandbox/1/overview)

---

This format should enhance readability and align with the Google Developer Style Guide.
