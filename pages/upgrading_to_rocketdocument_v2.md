This guide will help you quickly upgrade from RocketDocument v1 to v2, ensuring your integration remains compatible with the latest version. Follow these steps to update your setup.
# Step 1: Update API Endpoints
Update all your RocketDocument API endpoints from v1 to v2. Here are some key examples:

| Endpoint Name | Old Endpoint | New Endpoint |
|-------------------|--------------------------------------------------------------------------------------------|--------------|
| [Create an interview](create_an_interview_reference)    | `POST /rocketdoc/v1/interviews`                             | `POST /rocketdoc/v2/interviews` |
| [Get a document](get_a_document_reference)             | `GET /rocketdoc/v1/documents/:documentId`                     | `GET /rocketdoc/v2/documents/:documentId`    |
| [Get interview](get_interview_reference)           | `GET /rocketdoc/v1/interviews/:interviewId`                              | `GET /rocketdoc/v2/interviews/:interviewId`             |
> **Note**: v2 endpoints provide more detailed information compared to v1. Explore these new endpoints to enhance the user experience.
> 
> **Note**: [Get interview](get_interview_reference) has small changes on response structure


# Step 2: Update Embedded UX URL
Change the RocketDocument Embedded UX URL from v1 to v2:

|   Environment    |                                         Old v1 URL                                         | New v2 URL |
|-------------------|--------------------------------------------------------------------------------------------|--------------|
| Sandbox | `https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js`| `https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js`  |
| Production | `https://rocket-document.rocketlawyer.com/rocket-document.js`  | `https://rocket-document.rocketlawyer.com/v2/rocket-document.esm.js`  |
# Step 3: Adapt the `<script>` Tag
When embedding the RocketDocument v2 Embedded UX, update the `<script>` tag as follows:
```html
<script 
  type="module"
  src="https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js">
</script>
```
# Step 4: Update Monitored Embedded UX Events
Adjust the monitored events in the Embedded UX. Access the [RocketDocument Events](link) for additional information on how to use events on your application.

RocketDocument v2 will fire both legacy v1 and new v2 events. However, starting January 2nd, 2025, only the new v2 events will be active. Ensure that your system is ready to handle these changes. 

By following these steps, you'll successfully upgrade to RocketDocument v2, taking advantage of the latest features and improvements. If you still have any questions, contact our Support Team [add email].
