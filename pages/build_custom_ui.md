# How to Build a Custom UI

This guide will help partners integrate their own UX with RocketDocument v2 API, focusing on direct API interactions. The integration involves obtaining access tokens, selecting document templates, starting and resuming interviews, iterating through question pages, and completing interviews to retrieve documents.

You will go through the following steps:

- [Step 1: Authenticate](#step-1-authenticate)
- [Step 2: Choose a Template](#step-2-choose-a-template)
- [Step 3: Start an Interview](#step-3-start-an-interview)
- [Step 4: Resume an Interview](#step-4-resume-an-interview)
- [Step 5: Iterate Through Question Pages](#step-5-iterate-through-question-pages)
- [Step 6: Complete the Interview and Get the Document](#step-6-complete-the-interview-and-get-the-document)

## Step 1: Authenticate

To begin interacting with the RocketDocument API, you must obtain a general access token. This token is used to authenticate most of the API requests, ensuring secure communication between your backend systems and the RocketDocument API.

### Create General Access Token
To interact with the RocketDocument API, you'll first need to obtain a general access token. This token will be used to authenticate most subsequent API requests.

**Endpoint:**
```
POST /v1/auth/accesstoken
```

**Request Body:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/partners/v1/auth/accesstoken' \
--header 'Content-Type: application/json' \
--data '{
	"grant_type": "client_credentials",
	"client_id": "{{partnerClientId}}",
	"client_secret": "{{partnerClientSecret}}"
}'
```

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

### Store Token
Store this token securely in your backend for use in the next requests.

## Step 2: Choose a Template

In this step, you will retrieve a list of available document templates from the RocketDocument API. You can also obtain a thumbnail image and an HTML preview for a specific template, allowing you to display template options within your custom UI.

### Get List of Templates
Retrieve a list of available document templates.

**Endpoint:**
```
GET /v2/templates
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates' \
--header 'Authorization: Bearer {{generalAccessToken}}'
```

**Response:**
```json
[
  {
    "id": "template-id-1",
    "name": "Template Name 1"
  },
  {
    "id": "template-id-2",
    "name": "Template Name 2"
  }
]
```

### Get Template Thumbnail
Get a thumbnail image for a specific template.

**Endpoint:**
```
GET /v2/templates/{{templateId}}/thumbnail
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates/{{templateId}}/thumbnail'
```

**Response:**
Image data

### Get Template Preview
Retrieve a preview of a specific template.

**Endpoint:**
```
GET /v2/templates/{{templateId}}/preview
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates/{{templateId}}/preview'
```

**Response:**
HTML content

## Step 3: Start an Interview

Initiate an interview session by selecting a document template and creating either a persistent or ephemeral interview. This session will guide users through a series of questions, collecting the necessary information to complete the document.

### Create Persistent Interview
Initiate a persistent interview session with a selected template.

**Endpoint:**
```
POST /v2/interviews
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{generalAccessToken}}' \
--data-raw '{
    "templateId": "04d9d0ba-3113-40d3-9a4e-e7b226a72154",
    "partnerEndUserId": "UNIVERSAL PARTY IDENTIFIER - UNIQUE ID TO REPRESENT A CUSTOMER",
    "partyEmailAddress": "someone@something.com"
}'
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

### Create Ephemeral Interview
Initiate an ephemeral interview session with a selected template.

**Endpoint:**
```

```

**Request:**
```curl

```

**Response:**
```json

```

### Get Scoped Access Token
Obtain a scoped access token for the interview session.

**Endpoint:**
```
POST /v1/auth/accesstoken
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/partners/v1/auth/accesstoken' \
--header 'Content-Type: application/json' \
--data '{
    "grant_type": "authorization_code",
	"client_id" : "{{partnerClientId}}",
	"client_secret" : "{{partnerClientSecret}}",
	"code" : "{{serviceToken}}"
}'
```

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
    "access_token": "scoped-access-token",
    "application_name": "eebb78c0-ab4f-4212-8665-b1292330dbf5",
    "scope": "",
    "expires_in": "35999",
    "refresh_count": "0",
    "status": "approved"
}
```

## Step 4: Resume an Interview

For persistent interviews, users can resume a previously started session. This step involves retrieving the saved state of the interview and continuing from where the user left off, using a scoped access token for secure interaction.

### Resume Interview
Resume an existing interview using the scoped access token and saved answers.

**Endpoint:**
```
PATCH /v2/interviews/{{interviewId}}
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}' \
--header 'Content-Type: application/json' \
--header 'Authorization: [[Authorization-masked-secret]]'
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

## Step 5: Iterate Through Question Pages

As users progress through the interview, you will retrieve and submit individual pages of questions. This step ensures that user responses are captured, stored, and used to generate the next page, guiding the user through the document creation process.

### Get First Page
Retrieve the first page of the interview session.

**Endpoint:**
```
GET /v2/interviews/{{interviewId}}/pages/first
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}/pages/first' \
--header 'Authorization: Bearer {{scopedAccessToken}}'
```

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
Retrieve a specific page of the interview session by its page ID.

**Endpoint:**
```
GET /v2/interviews/{{interviewId}}/pages/{{pageId}}
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}/pages/{{pageId}}' \
--header 'Authorization: Bearer {{scopedAccessToken}}' \
--header 'Cookie: _pxhd=xj5E0ayjUYJIzrDGYVLh6P6xUWtN8weR0Bk8gq8i7XPTkiVTH2TJXqWfA/lCQN0gmeU8LhnPX-pwdgUOR2pb7A==:WGyRNLXQcM9GDevBCpCKJm0H23RMzcKj0Pain3jxm6Y/6lEpBdgDs1Afk9g7B8f3eB5ZVmpZMvrxMPJP-1KjpmgcQuNEXrbg12rwKYO6JDI='
```

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
Submit the current page and retrieve the next page preview.

**Endpoint:**
```
PATCH /v2/interviews/{{interviewId}}/pages/{{pageId}}
```

**Request:**
```curl
curl --location --request PATCH 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}/pages/{{pageId}}' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{scopedAccessToken}}' \
--data '{
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
  "answersPayload": ...
}'
```

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
    }
}
```

## Step 6: Complete the Interview and Get the Document

Once all questions are answered, the interview session is completed, and the final document is generated. You can then retrieve the completed document, either as a persistent document linked to the session or as an ephemeral document based on the session's data.

### Complete Interview
Complete the interview session and generate the final document.

**Endpoint:**
```
POST /v2/interviews/{{interviewId}}/completions
```

**Request:**
```curl
curl --location --request POST 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews/{{interviewId}}/completions' \
--header 'Authorization: Bearer {{generalAccessToken}}'
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
Retrieve the completed persistent document.

**Endpoint:**
```
GET /v2/documents/{{documentID}}
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/documents/{{documentID}}' \
--header 'rl-binder-id: 1dfb6084-e1f3-4229-9155-18f5a4a7b335' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{generalAccessToken}}'
--header 'Cookie: _pxhd=xj5E0ayjUYJIzrDGYVLh6P6xUWtN8weR0Bk8gq8i7XPTkiVTH2TJXqWfA/lCQN0gmeU8LhnPX-pwdgUOR2pb7A==:WGyRNLXQcM9GDevBCpCKJm0H23RMzcKj0Pain3jxm6Y/6lEpBdgDs1Afk9g7B8f3eB5ZVmpZMvrxMPJP-1KjpmgcQuNEXrbg12rwKYO6JDI='
```

**Response:**
Document data

### Get Ephemeral Document
Retrieve the completed ephemeral document.

**Endpoint:**
```
POST /v2/documents
```

**Request:**
```curl
curl --location 'https://api-sandbox.rocketlawyer.com/rocketdoc/v2/documents' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{generalAccessToken}}' \
--header 'Cookie: _pxhd=xj5E0ayjUYJIzrDGYVLh6P6xUWtN8weR0Bk8gq8i7XPTkiVTH2TJXqWfA/lCQN0gmeU8LhnPX-pwdgUOR2pb7A==:WGyRNLXQcM9GDevBCpCKJm0H23RMzcKj0Pain3jxm6Y/6lEpBdgDs1Afk9g7B8f3eB5ZVmpZMvrxMPJP-1KjpmgcQuNEXrbg12rwKYO6JDI=' \
--data '{
  "interviewId": "43b02054-85ba-4282-89e3-650be4e34fd3",
  "mimeType": "text/html",
  "answersPayload": ...
}'
```

**Response:**
```json

```
