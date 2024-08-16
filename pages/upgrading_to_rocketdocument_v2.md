This guide is designed to help you seamlessly upgrade from RocketDocument v1 to v2, ensuring that your integration stays compatible with the latest features and improvements. It is specifically tailored for users of the embedded RocketDocument UX component. 

Please note that other APIs related to authentication, binders, sign, and events will remain on version 1 (v1) and should continue to be accessed through their v1 endpoints. Follow the steps outlined below to update your setup effectively.

> **Tip**: Check the [Release Notes](/pages/release_notes.md) for the latest features and enhancements available in the v2 API.

## Step 1: Update API Endpoints

Update all your RocketDocument API endpoints from v1 to v2. Here are some key examples:

| Endpoint Name | Old Endpoint | New Endpoint |
|-------------------|--------------------------------------------------------------------------------------------|--------------|
| [Create an interview](create_an_interview_reference)    | `POST /rocketdoc/v1/interviews`                             | `POST /rocketdoc/v2/interviews` |
| [Get a document](get_a_document_reference)             | `GET /rocketdoc/v1/documents/:documentId`                     | `GET /rocketdoc/v2/documents/:documentId`    |
| [Get interview](get_interview_reference)           | `GET /rocketdoc/v1/interviews/:interviewId`                              | `GET /rocketdoc/v2/interviews/:interviewId`             |

> **Important Update**: We've tried to keep the API backward compatible, but the response structure for [Get Interview](get_interview_reference) has changed in v2. This is a necessary breaking change, so please update your integration accordingly.

## Step 2: Update Embedded UX URLs and Script

To switch to RocketDocument v2, update the Embedded UX URL used in the `<script>` tag in your integration. This ensures that your application loads the correct version of the RocketDocument component. The following table presents the new and old environment variables: 
   
|   Environment    |                                         Old v1 URL                                         | New v2 URL |
|-------------------|--------------------------------------------------------------------------------------------|--------------|
| Sandbox | `https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js`| `https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js`  |
| Production | `https://rocket-document.rocketlawyer.com/rocket-document.js`  | `https://rocket-document.rocketlawyer.com/v2/rocket-document.esm.js`  |

Update your HTML's `<script>` tag to point to the new v2 URL. The following code block presents an example of using the new Sandbox environment URL:

```html
<script 
  type="module"
  src="https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js">
</script>
```

## Step 3: Update Monitored Embedded UX Events

To ensure compatibility with RocketDocument v2, it's important to update the events monitored by your Embedded UX component. The v2 version introduces new events and deprecates some of the old ones.

RocketDocument v2 will initially fire both legacy v1 events and new v2 events, but starting January 2nd, 2025, only the new v2 events will remain active. Here are the key changes you need to address:

- The `interview-started` event has been deprecated and should be replaced with the `interview-loaded` event.
- The `rocketdocumenterror` event has been deprecated in favor of the more accurately named `interview-error` event.

Ensure that your system is updated to handle these new v2 events to maintain functionality beyond the deprecation date.

> **Additional Information**: For more detailed information on how to implement and use these events, refer to the [RocketDocument Events](/pages/ux-component-events.md) documentation.

By following these steps, you'll successfully upgrade to RocketDocument v2, taking advantage of the latest features and improvements. If you have any questions, please contact our Support Team [add email].

