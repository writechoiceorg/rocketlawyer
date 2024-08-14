This guide will help partners integrate their own UX with RocketDocument v2 API, focusing on direct API interactions. The integration involves obtaining access tokens, selecting document templates, starting interviews, going through question pages, and completing interviews to retrieve documents.

You will go through the following steps:

- [Step 1: Authenticate](#step-1-authenticate)
- [Step 2: Choose a Template](#step-2-choose-a-template)
- [Step 3: Start an Interview](#step-3-start-an-interview)
- [Step 4: Navigating Question Pages](#step-4-navigating-question-pages)
- [Step 5: Complete the Interview and Get the Document](#step-5-complete-the-interview-and-get-the-document)

## Step 1: Authenticate

To begin interacting with the RocketDocument API, you must obtain a general access token. This token authenticates most API requests, ensuring secure communication between your backend systems and the RocketDocument API.

### Create General Access Token
To interact with the RocketDocument API, you'll first need to obtain a general access token. To create a token, you will use the [Create General Access Token](link) endpoint. Below is an example of a request for creating a token:

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

## Step 2: Choose a Template

In this step, you will retrieve a list of available document templates from the RocketDocument API. If you want, you can also obtain a thumbnail image and an HTML preview for a specific template, allowing you to display template options within your custom UI.

### Get List of Templates
Retrieve a list of available document templates by sending a request to the [Get Template List](link) endpoint. Below is an example of request for retrieving the list of templates:

**Request:**
```curl
curl --request GET \
     --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates' \
     --header 'Authorization: Bearer {{generalAccessToken}}'
```

This endpoint will return a list of templates with the `templateId` and `templateName`, which you will need to retrieve the desired template later. Below, you will find an example of a response:

**Response:**
```json
[
  {
    "templateId": "0bf17ad8-a229-1289-ef4a-00b7c9ad9cab",
    "templateName": "Employment contract",
    "shortDescription": "A contract for employment.",
    "longDescription": "A contract for employment with options for employer & employee.",
    "medianMinutesToComplete": 10,
    "type": "standard",
    "memberOf": [
      "5e281c0e-c7fc-4b32-85d9-d2c9c59c8aa5",
      "4b120b4d-b6fc-4b32-85d9-a1c9c59c8aa4"
    ],
    "thumbnailRef": "https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates/0bf17ad8-a229-1289-ef4a-00b7c9ad9cab/thumbnail",
    "previewRef": "https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates/0bf17ad8-a229-1289-ef4a-00b7c9ad9cab/preview"
  }
]
```

### Optional steps

You can also add additional information to the template selection to improve your user experiece.

- Get a thumbnail image for a specific template by submitting a request to the [Retrieve a Termplate Thumbnail](link) endpoint.
- Retrieve a preview of a specific template by submitting a request to the [Retrieve a Template Preview](link) endpoint.

## Step 3: Start an Interview

Initiate an interview session by selecting a document template and creating either a persistent or ephemeral interview. In both cases, the user will fill in the preexisting fields of the chosen template to generate a personalized document.

### Create Persistent Interview
When using the persistent interview, the answers to each question are stored in RocketLawyer's servers and retrieved when the interview finishes to create the document. To start a persistent interview, use the [Create an Interview](link) endpoint, using the following model of request:

**Request:**
```curl
curl --request POST \
     --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {{generalAccessToken}}' \
     --data-raw '
{
    "templateId": "04d9d0ba-3113-40d3-9a4e-e7b226a72154",
    "partnerEndUserId": "UNIVERSAL PARTY IDENTIFIER - UNIQUE ID TO REPRESENT A CUSTOMER",
    "partyEmailAddress": "someone@something.com"
}
'
```

In this request, you will send the selected template's identifier (`templateId`), the user identifier (`partnerEndUserId`), and their email address (`partyEmailAddress`). Learn more about each object in the [Concepts page](/pages/glossary).

**Response:**
```json
{
    "interviewName": "Lease Agreement (6)",
    "concurrencyId": "78f97e2f-f947-4441-bc71-1a08e9e5ec7c",
    "partnerEndUserId": "UNIVERSAL PARTY IDENTIFIER - UNIQUE ID TO REPRESENT A CUSTOMER",
    "storageType": "persistent",
    "interviewStatus": "created",
    "createdAt": "2024-08-08T19:21:43.112Z",
    "updatedAt": "2024-08-08T19:21:43.112Z",
    "answersPayload": {
        "Fkrqobtz86esyj": "false",
        "Fkro0blo0rqrgk": "",
        ...
    },
    "binder": {
        "binderId": "1dfb6084-e1f3-4229-9155-18f5a4a7b335",
        "documentId": "83a61e13-e2c1-44db-a9cb-44ee1a025fa5"
    },
    "interviewId": "43b02054-85ba-4282-89e3-650be4e34fd3",
    "templateId": "04d9d0ba-3113-40d3-9a4e-e7b226a72154",
    "templateVersionId": "9f0fd7b2-b53c-49a7-aedb-1d5f01d41ced"
}
```

In the response, you will receive the following information:

- `storageType`: The confirmation of the type of interview, in this case, persistent.
- `answersPayload`: A key-value object for each answer. Each key refers to an answer unique identifier in the system, with its value being the actual answer provided by the end user.
- `binderId`: The unique identifier of the Binder to which the interview will be added. This object is only used in persistent interviews.
- `documentId`: The document's unique identifier. This object is only used in persistent interviews.
- `interviewId`: The interview's unique identifier.
- `templateId`: The template's unique identifier.
- `templateVersionId`: The template version identification.

### Create Ephemeral Interview
When using an ephemeral interview, the answers are not kept in RocketLawyer's servers, so they must all be sent once the interview is done to create the document. To start an ephemeral interview, use the [Create an Interview](link) endpoint, using the following model of request:

**Request:**
```curl
curl --request POST \
     --url https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews \
     --header 'accept: application/json' \
     --header 'content-type: application/json' \
     --data '
{
    "templateId": "{{templateId}}",
    "partyEmailAddress": "someone@something.com",
    "storageType": "ephemeral"
}
'
```

For the ephemeral interview, you will add the `"storageType": "ephemeral"` object to the request. The response will be similar to the persistent interview but will return an ephemeral `storageType` and no `binderId` or `documentId`.

**Response:**
```json
{
    "interviewName": "Lease Agreement (8)",
    "pageId": "first",
    "partnerEndUserId": "UNIVERSAL PARTY IDENTIFIER - UNIQUE ID TO REPRESENT A CUSTOMER",
    "storageType": "ephemeral",
    "interviewStatus": "created",
    "createdAt": "2024-08-09T21:16:45.573Z",
    "updatedAt": "2024-08-09T21:16:45.573Z",
    "answersPayload": {
        "Fkrqobtz86esyj": "false",
        "Fkro0blo0rqrgk": "",
        ...
    },
    "interviewId": "b5ac7079-7543-4870-92c3-172c9d2d7928",
    "templateId": "04d9d0ba-3113-40d3-9a4e-e7b226a72154",
    "templateVersionId": "9f0fd7b2-b53c-49a7-aedb-1d5f01d41ced"
}
```

### Get Scoped Access Token
To access the pages related to a specific `documentId`, you will need a Scoped Access Token. Having this token to use for front-end matters will keep your general access token safe on your back end.

#### Create Service Token

To create a scoped access token, you will first need a service token. You can get it by sending a request with the following parameters to the authentication endpoint.

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

#### Create Scoped Access Token

Now you can create a scoped access token, using the service token as an authenticator for the request, as exemplified below:

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

The response will return the scoped access token under the `access_token` object.

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

## Step 4: Navigating Question Pages

As users progress through the interview, you will retrieve and submit individual pages of questions. This step ensures that user responses are captured, stored, and used to generate the next page, guiding the user through the document creation process.

### Get First Page
Retrieve the first page of the interview session through the [Retrieve a Page](link) endpoint. To retrieve the first page, you will add the `pageId` parameter in the path as "first".

> **Ephemeral Interview**
> This is the only get page request that you will use for ephemeral interviews, since it will always return the template's default `answersPayload`.

Below, you can find an example request for the first page:

**Request:**
```curl
curl --request GET \
     --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}/pages/first' \
     --header 'Authorization: Bearer {{scopedAccessToken}}'
```

This will return a response with the questions for the first page, and the `isFirst` object will return `true`.

**Response:**
```json
{
    "answersPayload": {
        "Ckrmq1s8ymxcgp": [
            {
                "Fkrmq0xbl3olho": ""
            }
        ],
        ...
    },
    "name": "Lease Agreement",
    "preview": {
        "mimeType": "text/html",
        "data": "..."
    },
    "previousPageData": {
        "pageId": "Pkrmp0p6bxdxoe",
        "format": "reference"
    },
    "currentPageData": {
        "pageId": "Pkrmp0p6bxdxoe",
        "format": "display",
        "type": "single",
        "progressPercentage": 0,
        "questions": [
            {
                "id": "Qkrmp0p64z0tuu",
                "title": "Let's get started with a little information about the parties. Is the landlord a company or an individual?",
                "hint": "The landlord is the party, usually the owner, who has legal control over the possession of certain property and who may lease such possession to another.  The landlord is sometimes referred to as the \"lessor.\"",
                "fields": [
                    {
                        "id": "Fkrmp0p66hd6c7",
                        "type": "RADIO",
                        "default": true,
                        "label": "A company"
                    },
                    {
                        "id": "Fkrmp2d44gs58o",
                        "type": "RADIO",
                        "default": "",
                        "label": "An individual"
                    }
                ],
                "help": ""
            }
        ],
        "answers": {
            "Fkrmp0p66hd6c7": true,
            "Fkrmp2d44gs58o": false
        },
        "isFirst": true
    },
    "nextPageData": {
        "pageId": "null",
        "format": "reference"
    },
    "concurrencyId": "78f97e2f-f947-4441-bc71-1a08e9e5ec7c"
}
```

### Get Page by ID
To retrieve a page by its ID, use the [Retrieve a Page](link) endpoint with the desired page's ID in the path `pageId` parameter. This is only possible for persistent interviews. Below, you will find an example of a request by page ID:

**Request:**
```curl
curl --request GET \
     --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}/pages/{{pageId}}' \
     --header 'Authorization: Bearer {{scopedAccessToken}}' \
```

This request will get a response similar to the one below:

**Response:**
```json
{
    "answersPayload": {
        "Ckrmq1s8ymxcgp": [
            {
                "Fkrmq0xbl3olho": ""
            }
        ],
        ...
    },
    "name": "Lease Agreement",
    "preview": {
        "mimeType": "text/html",
        "data": "..."
    },
    "previousPageData": {
        "pageId": "Pkrmp0p6bxdxoe",
        "format": "reference"
    },
    "currentPageData": {
        "pageId": "Pks6p0qrdiwvbf",
        "format": "display",
        "type": "single",
        "progressPercentage": 1,
        "questions": [
            {
                "id": "Qkrmpkupdijh2k",
                "title": "Who is the landlord?",
                "hint": "",
                "fields": [
                    {
                        "id": "Fkrmpkupg2ces0",
                        "type": "TEXT",
                        "default": "",
                        "label": "Company Name"
                    },
                    ...
                ]
            }
        ],
        "answers": {
            "Fkrmpkupg2ces0": "",
            "Fkrmpmfcyndisl": "",
            ...
        },
        "isFirst": false
    },
    "nextPageData": {
        "pageId": "null",
        "format": "reference"
    }
}
```

### Submit Page and Display Next

Once a page is filled, you can submit the questions on the current page and move on to the next by submitting a request to the [Update an Interview Page's Answers](link). With this endpoint, you can save and go to either the previous, same, or next page. In the following example, you see a request for the next page:

**Request:**
```curl
curl --request PATCH \
     --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/42313c4d-11bb-414a-81a4-9fda067d1ae7/pages/Pkrmp0p6bxdxoe' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {{scopedAccessToken}}' \
     --data '
{
  "currentPageData": {
    "format": "reference"
  },
  "previousPageData": {
    "format": "reference"
  },
  "nextPageData": {
    "format": "display"
  },
  "preview": {
    "mimeType": "text/html"
  },
  "answersPayload": {"Ckrmq1s8ymxcgp":[{"Fkrmq0xbl3olho":""}],...}
}
'
```

In this request, the page objects (`currentPageData`, `previousPageData`, and `nextPageData`) can be either `reference` or `display`. When sending `reference`, the response will return only the `pageId` to help with navigation. When using `display`, the response will return complete question information for that page, including all content necessary for the interview process, which you see in the example response below:

**Response:**
```json
{
    "answersPayload": {
        "Ckrmq1s8ymxcgp": [
            {
                "Fkrmq0xbl3olho": ""
            }
        ],
        ...
    },
    "name": "Lease Agreement",
    "preview": {
        "mimeType": "text/html",
        "data": "..."
    },
    "previousPageData": {
        "pageId": "Pkrmp0p6bxdxoe",
        "format": "reference"
    },
    "currentPageData": {
        "pageId": "Pkrmp0p6bxdxoe",
        "format": "reference"
    },
    "nextPageData": {
        "pageId": "Pks6p0qrdiwvbf",
        "format": "display",
        "type": "single",
        "progressPercentage": 1,
        "questions": [
            {...}
        ],
        "answers": {...},
        "isFirst": false
    }
}
```

### Optional step: Resume an Interview

For persistent interviews, users can resume a previously started session. This step involves retrieving the saved state of the interview and continuing from where the user left off. You can do that  by submitting a request to the [Retrieve Interview by ID](link) endpoint. Below, you will find an example of request and response for this endpoint:

**Request:**
```curl
curl --request GET \
     --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}' \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {{scopedAccessToken}}'
```

**Response:**
```json
{
    "interviewName": "Lease Agreement (6)",
    "concurrencyId": "78f97e2f-f947-4441-bc71-1a08e9e5ec7c",
    "partnerEndUserId": "UNIVERSAL PARTY IDENTIFIER - UNIQUE ID TO REPRESENT A CUSTOMER",
    "storageType": "persistent",
    "interviewStatus": "created",
    "createdAt": "2024-08-08T19:21:43.112Z",
    "updatedAt": "2024-08-08T19:21:43.505Z",
    "answersPayload": {
        "Ckrmq1s8ymxcgp": [
            {
                "Fkrmq0xbl3olho": ""
            }
        ],
        ...
    },
    "binder": {
        "binderId": "1dfb6084-e1f3-4229-9155-18f5a4a7b335",
        "documentId": "83a61e13-e2c1-44db-a9cb-44ee1a025fa5"
    },
    "interviewId": "43b02054-85ba-4282-89e3-650be4e34fd3",
    "templateId": "04d9d0ba-3113-40d3-9a4e-e7b226a72154",
    "templateVersionId": "9f0fd7b2-b53c-49a7-aedb-1d5f01d41ced"
}
```

## Step 5: Complete the Interview and Get the Document

Once all questions are answered, the interview session is completed, and the final document is generated. You can then retrieve the finished document as a persistent document linked to the `documentId` or an ephemeral document based on the session's data.

### Complete Interview
Complete the interview session and ensure the processing and saving of the answers by sending a request to the [Complete an Interview](link) endpoint. Below, you will find an example of a request and a response:

**Request:**
```curl
curl --request POST \
     --url https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/interviewId/completions \
     --header 'accept: application/json' \
     --header 'authorization: Bearer {{generalAccessToken}}'
```

**Response:**
```json
{
    "binder": {
        "binderId": "1dfb6084-e1f3-4229-9155-18f5a4a7b335",
        "documentId": "83a61e13-e2c1-44db-a9cb-44ee1a025fa5"
    }
}
```

### Get Persistent Document
To retrieve the completed persistent document, make a request to the [Retrieve Persistent Document](link) endpoint. It can return a document in either HTML or PDF format, depending on what is informed in the `Accept` parameter. Below, you will find an example of a request:

**Request:**
```curl
curl --request GET \
     --url https://api-sandbox.rocketlawyer.com/rocketdoc/v2/documents/83a61e13-e2c1-44db-a9cb-44ee1a025fa5 \
     --header 'Accept: text/html' \
     --header 'authorization: Bearer {{generalAccessToken}}'
```

**Response:**
Document data in HTML or PDF format.

### Get Ephemeral Document
To retrieve the completed ephemeral document, make a request to the [Retrieve Ephemeral Document](link) endpoint. This will submit all the questions and answers linked to the `interviewId` and return the completed document. Below, you will find an example of a request and a response:

**Request:**
```curl
curl --request POST \
     --url https://api-sandbox.rocketlawyer.com/rocketdoc/v2/documents \
     --header 'accept: application/json' \
     --header 'authorization: Bearer {{generalAccessToken}}' \
     --header 'content-type: application/json' \
     --data '
{
  "mimeType": "text/html",
  "answersPayload": {
    "Ckrmq1s8ymxcgp": "True",
    ...
  },
  "interviewId": "76105b3e-ac8a-4202-8ed2-b991a02b8456"
}
'
```

**Response:**
```json
{
  "interviewId": "76105b3e-ac8a-4202-8ed2-b991a02b8456",
  "storageType": "ephemeral",
  "document": {
    "mimeType": "text/html",
    "data": "PGh0bWw+CiAgPGJvZHk+CiAgICA8cD5Mb3JlbSBpcHN1bSBkb2xvciBzaXQgYW1ldCwgY29uc2VjdGV0dXIgYWRpcGlzY2luZyBlbGl0LjwvcD4KICA8L2JvZHk+CjwvaHRtbD4K"
  }
}
```
