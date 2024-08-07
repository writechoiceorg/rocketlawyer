This Quick Start guides you through using the **RocketDocument™ Embedded UX** to create and customize a Rocket Lawyer document through a simple interview-style experience. By following these steps, you'll be able to complete a document and display it using the **RocketSign® Embedded UX**.


## What You'll Need

1. **Client Credentials**  
   Obtain these through the onboarding process in the [Welcome Guide](welcome-guide).

2. **templateId**  
   This is the unique ID for the template used to initialize the interview. For example, for a lease agreement, you can use: `04d9d0ba-3113-40d3-9a4e-e7b226a72154`.

3. **partnerEndUserId**  
   An ID that identifies the end user in your system.

4. **partyEmailAddress**  
   The email address of the end user for document notifications.

## Getting Started

### Step 1: Generate an Access Token

> **Note:** Use `api-sandbox.rocketlawyer.com` for testing. For production, switch to `api.rocketlawyer.com`.

Authenticate each call to the **RocketDocument API** by obtaining an Access Token. Call the [Authentication API](/docs/partner-auth-service-product-sandbox/1/routes/accesstoken/post) as follows:

```http
POST https://api-sandbox.rocketlawyer.com/partners/v1/auth/accesstoken
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

> **Note:** Token content is redacted for security.

### Step 2: Create the Interview

> **Assumptions:**
> - You have the `templateId` for the template to base the interview on. In this guide, we're using a Lease Agreement templateId. For other document types, use the corresponding templateId.
> - You have a valid `partnerEndUserId`.

**Request**

Create an interview by making a POST request to the [RocketDocument API](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/post):

```http
POST https://api-sandbox.rocketlawyer.com/rocketdoc/v1/interviews
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

To embed RocketDocument UX in your UI, include the following in your HTML:

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

> Attention! `{interview-id}` is not the same thing as `templateId`.

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

After loading RocketDocument Embedded UX, your interview should be interactive:

![RocketDocument Embedded UX](https://rl-cicdv2-apigee-public-prod.apigee.io/files/RocketDocument-Embedded-Mobile.png)

> Congratulations! You've successfully displayed the document using **RocketDocument Embedded UX**.

### Step 5 (Optional): Integrate with **RocketSign**

To integrate with **RocketSign**, retrieve the Interview JSON Object and save the **binderId**. Keep the service token (`rl-rdoc-servicetoken`) for reuse with RocketSign.

**Request**

```http
GET https://api-sandbox.rocketlawyer.com/rocketdoc/v1/interviews/{interviewId}
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

Now that you can create, display, and interact with a document interview, explore these resources:

**Quick Starts**
- [Quick Start: RocketSign Embedded UX](rocketsign-embedded-ux)

**API Documentation**
- [RocketDocument API Documentation](/docs/rocketdoc-api-product-sandbox/1/overview)
- [Authentication API Documentation](/docs/partner-auth-service-product-sandbox/1/overview)

---

This format should enhance readability and align with the Google Developer Style Guide.
