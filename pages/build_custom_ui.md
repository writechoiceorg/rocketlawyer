# How to Build a Custom UI

This guide will help partners integrate their own UX with RocketDocument v2 API, focusing on direct API interactions. The integration involves obtaining access tokens, selecting document templates, starting and resuming interviews, iterating through question pages, and completing interviews to retrieve documents.

## Authentication

### Create General Access Token
To interact with the RocketDocument API, you'll first need to obtain a general access token. This token will be used to authenticate subsequent API requests.

**Endpoint:**
```
POST /v1/auth/accesstoken
```

**Request Body:**
```json
{
  "grant_type": "client_credentials",
  "client_id": "{{partnerClientId}}",
  "client_secret": "{{partnerClientSecret}}"
}
```

**Response:**
```json
{
  "access_token": "your-general-access-token"
}
```

### Store Token
Store this token securely in your backend for use in subsequent requests.

## Choosing a Template

### Get List of Templates
Retrieve a list of available document templates.

**Endpoint:**
```
GET /v2/templates
```

**Headers:**
```http
Authorization: Bearer {{generalAccessToken}}
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

**Headers:**
```http
Authorization: Bearer {{generalAccessToken}}
```

**Response:**
Image data

### Get Template Preview
Retrieve a preview of a specific template.

**Endpoint:**
```
GET /v2/templates/{{templateId}}/preview
```

**Headers:**
```http
Authorization: Bearer {{generalAccessToken}}
```

**Response:**
HTML content

## Starting an Interview

### Create Interview
Initiate an interview session with a selected template.

**Endpoint:**
```
POST /v2/interviews
```

**Headers:**
```http
Authorization: Bearer {{generalAccessToken}}
```

**Request Body:**
```json
{
  "templateId": "{{templateId}}",
  "partnerEndUserId": "{{upid}}",
  "partyEmailAddress": "someone@something.com"
}
```

**Response:**
```json
{
  "interviewId": "new-interview-id",
  "answersPayload": { ... }
}
```

### Get Scoped Access Token
Obtain a scoped access token for the interview session.

**Endpoint:**
```
POST /v1/auth/accesstoken
```

**Headers:**
```http
Authorization: Bearer {{serviceToken}}
```

**Request Body:**
```json
{
  "grant_type": "authorization_code",
  "client_id": "{{partnerClientId}}",
  "client_secret": "{{partnerClientSecret}}",
  "code": "{{serviceToken}}"
}
```

**Response:**
```json
{
  "access_token": "scoped-access-token"
}
```

## Resuming an Interview

### Resume Interview
Resume an existing interview using the scoped access token and saved answers.

**Endpoint:**
```
PATCH /v2/interviews/{{interviewId}}
```

**Headers:**
```http
Authorization: Bearer {{scopedAccessToken}}
```

**Request Body:**
```json
{
  "answersPayload": { ... }
}
```

**Response:**
```json
{
  "answersPayload": { ... }
}
```

## Iterating Through Question Pages

### Get First Page
Retrieve the first page of the interview session.

**Endpoint:**
```
GET /v2/interviews/{{interviewId}}/pages/first
```

**Headers:**
```http
Authorization: Bearer {{scopedAccessToken}}
```

**Response:**
```json
{
  "pageId": "first-page-id",
  "questions": [ ... ],
  "answers": { ... }
}
```

### Get Page by ID
Retrieve a specific page of the interview session by its page ID.

**Endpoint:**
```
GET /v2/interviews/{{interviewId}}/pages/{{pageId}}
```

**Headers:**
```http
Authorization: Bearer {{scopedAccessToken}}
```

**Response:**
```json
{
  "pageId": "specific-page-id",
  "questions": [ ... ],
  "answers": { ... }
}
```

### Submit Page and Display Next
Submit the current page and retrieve the next page preview.

**Endpoint:**
```
PATCH /v2/interviews/{{interviewId}}/pages/{{pageId}}
```

**Headers:**
```http
Authorization: Bearer {{scopedAccessToken}}
```

**Request Body:**
```json
{
  "currentPageData": {
    "format": "display"
  },
  "answersPayload": { ... }
}
```

**Response:**
```json
{
  "nextPageData": {
    "pageId": "next-page-id",
    "questions": [ ... ],
    "answers": { ... }
}
```

## Completing the Interview and Getting the Document

### Complete Interview
Complete the interview session and generate the final document.

**Endpoint:**
```
POST /v2/interviews/{{interviewId}}/completions
```

**Headers:**
```http
Authorization: Bearer {{generalAccessToken}}
```

**Response:**
```json
{
  "documentId": "final-document-id"
}
```

### Get Document
Retrieve the completed document.

**Endpoint:**
```
POST /v2/documents
```

**Headers:**
```http
Authorization: Bearer {{generalAccessToken}}
```

**Request Body:**
```json
{
  "interviewId": "{{interviewId}}",
  "answersPayload": { ... }
}
```

**Response:**
Document data
