This page provides a step-by-step process for creating and customizing a Rocket Lawyer document using a simple interview-style experience. Following these steps, you can complete and display a document using the **RocketDocument Embedded UX**.
# Items Required
Ensure you have the following items ready to create and customize a Rocket Lawyer document:
- **Client Credentials**: Obtain these during the onboarding process, as detailed in the [Welcome Guide](welcome-guide).
- **templateId**: This is the unique ID for the template used to initialize the interview. For this guide, we'll use the lease agreement with the templateID `04d9d0ba-3113-40d3-9a4e-e7b226a72154`.
> **Environment Setup:** Use `api-sandbox.rocketlawyer.com` for testing environments. When you're ready for production, switch to `api.rocketlawyer.com`. This applies to all interactions with the RocketDocument API.
# Overview
Below are the links to all the steps on this page. Use them to navigate to the desired section.
1. [Generate an Access Token](#step-1-generate-an-access-token)
2. [Create the Interview](#step-2-create-the-interview)
3. [Access the RocketDocument UI](#step-3-access-the-rocketdocument-ui)
4. [Display Your Document](#step-4-display-your-document)
5. [Handle Interview Completion](#step-5-handle-interview-completion)
# Step 1: Generate an Access Token
Before interacting with the RocketDocument API, you must authenticate your calls by obtaining an Access Token. You will use this token to authorize all subsequent API requests. In this step, you will learn how to generate an Access Token by calling the Authentication API.
Authenticate each call to the **RocketDocument API** by obtaining an Access Token. Call the [Authentication API](/docs/partner-auth-service-product-sandbox/1/routes/accesstoken/post) as follows:
```http
POST https://api-sandbox.rocketlawyer.com/partners/v1/auth/accesstoken
```
Include the credentials (`client_id` and `client_secret`) and `grant_type`:
```json
{
  "client_id": "{api-key}",
  "client_secret": "{api-secret}",
  "grant_type": "client_credentials"
}
```
You will receive a response that includes an Access Token:
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.(content redacted).tBX73KTTjopBSRDL0cIBt3EK_DcA3Jc9KKonbpBn6HE"
}
```
> **Security Reminder:** Token content has been redacted for security reasons.
# Step 2: Create the Interview
With the Access Token in hand, the next step is to create an interview session for the document. This interview is based on a specific document template, which gathers the necessary information to customize the document. This step will guide you through making a POST request to the RocketDocument API to create the interview.

**Requirements**

Before proceeding, make sure to meet the following requirements:
- Have the `templateId` for the template on which to base the interview. This guide uses a Lease Agreement `templateId`. For other document types, use the corresponding `templateId`.
- Have a valid `partnerEndUserId`.

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
# Step 3: Access the RocketDocument UI
Now that the interview is set up, you need to embed the RocketDocument UX into your platform’s UI. This step involves adding specific HTML elements and JavaScript to load the interactive interview interface. Here, you will learn to include the required script and web component tags in your HTML.
> **JavaScript Event Notification:** JavaScript events will notify your front end of critical updates and let you know when it is safe to deactivate the module.
Add the script tag presented in the following code block in the page header:
```javascript
<script
  type="module"
  src="https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js">
</script>
```
Insert this web component tag in the body:
```html
<rocket-document serviceToken="{rl-rdoc-servicetoken}" interviewId="{interview-id}"></rocket-document>
```
The elements in this tag are:
- `{rl-rdoc-servicetoken}`: The service token from Step 1. Learn more about it in the [Authentication API Documentation](/docs/partner-auth-service-product-sandbox/1/overview).
- `{interview-id}`: From the response in Step 2.
> **Important Distinction:** `{interview-id}` is the unique identifier for a specific interview session, while `templateId` refers to the ID of the document template used to initiate that interview.
Simplified webpage example:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>RocketDocument Demo</title>
    <script
         type="module"
         src="https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js">
    </script>
  </head>
  <body>
    <div class="content">
      <rocket-document
        serviceToken="..."
        interviewId="...">
      </rocket-document>
    </div>
  </body>
</html>
```
# Step 4: Display Your Document
After loading **RocketDocument Embedded UX**, your interview should be interactive. Make sure to access the visual representation of the embedded interface, as shown in the image below. You can check the [RocketDocument v2 UX Events](/rocketdocument-v2-ux-events) page for additional information about checking the triggered events.

![RocketDocument Embedded UX](https://rl-cicdv2-apigee-public-prod.apigee.io/files/RocketDocument-Embedded-Mobile.png)
> **Success!** You've successfully displayed the document using **RocketDocument Embedded UX**.
# Step 5: Handle Interview Completion
After completing the interview process, the **RocketDocument Embedded UX** component will fire an `interview-completed` event. This will also tell the `ParentUI` that no further processes are running or have yet to be completed. Since the RocketDocument only fires the event, it is up to the `ParentUI` to take the user to the next screen.
Add the JavaScript code below to listen to the event:
```javascript
  <script>
    document
      .querySelector("rocket-document")
      .addEventListener("interview-completed", (e) => {
        console.log("interview-loading event received", e.detail);
      });
  </script>
```
For additional information, see the [RocketDocument v2 UX Events](/rocketdocument-v2-ux-events) page.
# Customizing and Controlling the Embedded UX Component
To customize the **RocketDocument Embedded UX** component to match your brand's look and feel, follow the steps below:
1. **Submit Your Style Preferences**: Fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSddJPPg0onclKYf2IIyRehCYwlTtlcogXXXxo0ZlwtZLd3ZZQ/viewform?fbzx=5836941555539130795) with your desired styles, including colors, logos, and button corner radius.
2. **Create a Request in the Partner Portal**: Once your style preferences are ready, submit a customization request through the [partner portal](https://rocket-lawyer.atlassian.net/servicedesk/customer/portal/10). The Rocket Lawyer team will make the changes to the component appearance.
These steps will ensure that the **RocketDocument Embedded UX** reflects your brand’s identity and provides a consistent user experience across your platform.

In addition, when using the **RocketDocument Embedded UX**, you can add arguments to the `<rocket-document>` tag to control the component behavior. The following code block presents an example of all arguments you can add to the tag:
```html
<rocket-document
  accessToken={access-token}
  serviceToken={rl-rdoc-servicetoken}
  interviewId={interview-id}
  pageId={page-ref}
  showProgressBar={true/false}
  showDocumentPreview={true/false}
  showDocumentTitle={true/false}>
</rocket-document>
```
You can notice that, in addition to the `accessToken`, `serviceToken`, and `interviewId` used for authentication and identification of the current interview, you can also use the following ones:
- `pageId`: Enables you to resume a previous interview the end user left. To use this feature, inform `pageId = "last"` to display the last answered page. 
- `showProgressBar`: Controls if the progress bar will be displayed. The default value is `true`.
- `showDocumentPreview`: Controls if the document preview is available. The default value is `true`.
- `showDocumentTitle`: Controls if the interview title will be displayed. The default value is `true`.

# Next Steps
You’ve successfully created, displayed, and interacted with a document interview. To further enhance your integration and explore additional capabilities, check out the following resources:
- [Quick Start: RocketSign Embedded UX](rocketsign-embedded-ux)
- [Authentication API Documentation](/docs/partner-auth-service-product-sandbox/1/overview)
- [RocketDocument v2 API Documentation](/docs/rocketdoc-api-product-sandbox/1/overview)
