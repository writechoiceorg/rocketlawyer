This page outlines the key changes in RocketDoc API v2, highlighting new features such as custom user experience options, enhanced data handling with Data I/O, and updates to embedded user experiences. It also details adjustments to API endpoints, additional response information, and improvements in event handling, providing a comprehensive overview for users familiar with v1 who are exploring the upgrades in v2.

# New Features

## Custom User Experience

The updates and new endpoints on RocketDocument API gives API partners the option to create a [custom user experience directly through the API](/rocket-document-v2-build-your-own-ui), allowing full customization, enabling partners to seamlessly reflect their brand identity and visual preferences within the user experience. 

In addition to persistent interviews (where Rocket Lawyer stores the interview data), you can also use ephemeral interviews, where no data is persisted on Rocket Lawyer servers.

## Data I/O

The [Data I/O feature](/rocket-document-v2-data-i/on) allows partners to input and extract data from interviews using [Tagged Answer Models (TAM)](/glossary). TAMs represent an structured way with entities and answers values on a document.  

### Benefits

- **Structured Data Retrieval:** Access the Tagged Answer Model (TAM) for any interview, allowing you to view, input and retrieve the entities and values of a document template questions that ususally the user answers.
- **Pre-filled Interviews:** Improve user experience by pre-filling interview fields with existing customer data, saving time and reducing redundancy.
- **Data Accessibility**: Easily access and retrieve user-provided data, enabling re-usage for new interviews and enhancing the similar document creation process.

# Additional Enhancements

## Embedded User Experience
We have completely rebuilt RocketDocument Embedded UX to work with the new v2 endpoints. However, the user experience remains largely similar for embedded users. Some adjustments were made to improve it, which you can see below:
- **Adjustments to Endpoints:** Endpoints have changed from the v1 format (`/rocketdoc/v1/interviews`) to the v2 format (`/v2/interviews`). Check the [Upgrading to RocketDocument v2](pages/upgrading_to_rocketdocument_v2) page for more information.
- **Changes to Events:** The list of events was updated. For more information, check the [UX Component Events](pages/ux-component-events) page.

## Detailed Document Template Information
With the v2 endpoint enhancements, partners can access:

- **List Templates**: Retrieve a customized list of templates available to each partner through the [Get Templates](https://rl-cicdv2-apigee-public-rocketdocv2.apigee.io/docs/rocketdoc-api-product-sandbox/1/routes/templates/get) endpoint. 
- **Median Time to Complete**: To display the estimated time required to complete the interview, based on data from thousands of Rocket Lawyer and partner customers over the past years. This helps end users understand how long it will take to complete the interview.
- **Short and Long Descriptions**: To provide clear document template descriptions to inform end users and ensure they have a solid understanding.
- **Thumbnails and Previews**: To enhance the user experience by presenting visual thumbnails and previews on interfaces.

## Resume Interviews
Partners can use the [Get Interview by ID](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%257BinterviewId%257D/get) endpoint to allow end users to resume their interviews from where they left off.


# Further Information

You can find more information on how to upgrage RocketDocument Embedded UX from v1 to v2 using the  in the [Guide: Upgrading to RocketDocument v2](/upgrading_to_rocketdocument_v2). You can also test the new v2 API through our [v2 Postman collection](link) and see the detailed [RocketDocument v2 API Reference](/docs/rocketdoc-api-product-sandbox/1/overview).
