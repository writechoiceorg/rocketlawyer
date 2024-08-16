This page outlines the key changes in RocketDoc API v2, highlighting new features such as custom user experience options, enhanced data handling with Data I/O, and updates to embedded user experiences. It also details adjustments to API endpoints, additional response information, and improvements in event handling, providing a comprehensive overview for users familiar with v1 who are exploring the upgrades in v2.

# New Features

## Custom User Experience

The updates and new endpoints on RocketDocument API allow API partners to build their own [custom UI](/rocket-document-v2-build-your-own-ui), replacing RocketDoc embedded UX. Using the RocketDocument API, partners can access all Rocket Lawyer features to create a completely customized user experience. As a result, partners can create a front end that is fully aligned with their brand identity and visual preferences. 

The RocketDocument API also allows partners to use ephemeral interviews. When partners use ephemeral interviews, no data persists on Rocket Lawyer servers, unlike in persistent interviews, in which Rocket Lawyer stores the interview data. Thus, partners can retain their own customer data and not share it with Rocket Lawyer. 

## Data I/O

The [Data I/O feature](/rocket-document-v2-data-i/on) allows partners to input and extract data from interviews using [Tagged Answer Models (TAM)](/glossary). TAMs represent a structured way with entities and answers values on a document.  

### Benefits

- **Structured Data Retrieval:** Access the Tagged Answer Model (TAM) for an interview. This allows you to view, input, and retrieve the entities and values of document template questions the user usually answers.
- **Pre-filled Interviews:** Improve user experience by pre-filling interview fields with existing customer data, saving time and reducing redundancy.
- **Data Accessibility**: Easily access and retrieve user-provided data, enabling re-usage for new interviews and enhancing the similar document creation process.

# Additional Enhancements

## Embedded User Experience
We have completely rebuilt RocketDocument Embedded UX to work with the new v2 endpoints. However, the user experience remains largely similar for embedded users. Some adjustments were made to improve it, which you can see below:
- **Adjustments to Endpoints:** Endpoints have changed from the v1 format (`/rocketdoc/v1/interviews`) to the v2 format (`/rocketdoc/v2/interviews`). Check the [Upgrading to RocketDocument v2](pages/upgrading_to_rocketdocument_v2) page for more information.
- **Changes to Events:** The number of events triggered by the Embedded UX increased. Now, you can follow each process step and handle it how it best suits you. For a complete overview of the available events access the [UX Component Events](pages/ux-component-events) page.

## Detailed Document Template Information
With the v2 endpoint enhancements, partners can access:

- **List Templates**: The [Get Templates] (https://rl-cicdv2-apigee-public-rocketdocv2.apigee.io/docs/rocketdoc-api-product-sandbox/1/routes/templates/get) endpoint allows you to retrieve a customized list of templates available to each partner. 
- **Template Grouping**: Now you can retrieve groups of templates based on customizable indexes. 
- **Median Time to Complete**: To display the estimated time required to complete the interview, based on data from thousands of Rocket Lawyer and partner customers over the past years. This helps end users understand how long it will take to complete the interview.
- **Short and Long Descriptions**: To provide clear document template descriptions to inform end users and ensure they have a solid understanding.
- **Thumbnails and Previews**: To enhance the user experience, visual thumbnails and previews are presented on interfaces.

## Resume Interviews
Partners can use the [Get Interview by ID](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%257BinterviewId%257D/get) endpoint to allow end users to resume their interviews from where they left off. For additional information, access [How to Build Your Own UI](/rocket-document-v2-build-your-own-ui)

# Further Information

You can find more information on upgrading RocketDocument Embedded UX from v1 to v2 using the [Guide: Upgrading to RocketDocument v2](/upgrading_to_rocketdocument_v2). You can also test the new v2 API through our [v2 Postman collection](link) and see the detailed [RocketDocument v2 API Reference](/docs/rocketdoc-api-product-sandbox/1/overview).
