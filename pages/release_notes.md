This release notes introduces key enhancements of RocketDocument v2. Rocket Lawyer has revamped the custom user experience, improved data handling with the new Data I/O feature, and upgraded the embedded UX. This release contains other enhancements, such as the ability for customers to resume interviews on the page where they left off. Read on to explore the full range of improvements and how they can benefit your integration with RocketDocument.


# New Features
## Custom User Experience
The updates and new endpoints on RocketDocument API allow API partners to build their own [custom UI](/rocket-document-v2-build-your-own-ui), replacing RocketDoc embedded UX. Using the RocketDocument API, partners can access all Rocket Lawyer features to create a completely customized user experience. As a result, partners can create a front end that is fully aligned with their brand identity and visual preferences. 
The RocketDocument API also allows partners to use ephemeral interviews. When partners use ephemeral interviews, no data persists on Rocket Lawyer servers, unlike in persistent interviews, in which Rocket Lawyer stores the interview data. 

## Data I/O

The [Data I/O feature](/rocket-document-v2-data-i/on) allows partners to input and extract data from interviews using [Tagged Answer Models (TAM)](/glossary). TAMs provide a structured method for representing entities and answer values within a document.


### Benefits
- **Structured Data Retrieval:** Access the Tagged Answer Model (TAM) for an interview. This allows you to view, input, and retrieve the values of document template questions the user answers.
- **Pre-filled Interviews:** Improve user experience by pre-filling interview fields with existing customer data, saving time and reducing redundancy.
- **Data Accessibility**: Easily access and retrieve user-provided data, enabling re-usage for new interviews and enhancing the similar document creation process.
# Additional Enhancements

## Embedded User Experience (UX)
Rocket Lawyer has completely rebuilt RocketDocument Embedded UX to work with the new v2 endpoints. However, the user experience remains similar for embedded users. Below is a list of improvements: 
- **Changes to Events**: The number of events triggered by the Embedded UX increased. Now, you can follow each process step and handle it as desired. Access the [UX Component Events](/rocketdocument-v2-ux-events) page for a complete overview of the available events.
- **Date Picker**: The date picker has been enhanced, making it easier and more intuitive for end users to select their desired date.
- **Processing**: The majority of logic processing was moved to Rocket Lawyer's backend, making the embedded component lighter and the process more secure. 

## Endpoints Adjustments
Endpoints have changed from the v1 format (`/rocketdoc/v1/interviews`) to the v2 format (`/rocketdoc/v2/interviews`). For more information, check the [Upgrading to RocketDocument v2](/upgrading-to-rocketdocument-v2) page.

## Detailed Document Template Information
With the v2 endpoint enhancements, partners can access:
- **List Templates**: The [Get Templates](https://rl-cicdv2-apigee-public-rocketdocv2.apigee.io/docs/rocketdoc-api-product-sandbox/1/routes/templates/get) endpoint allows you to retrieve a customized list of templates available to each partner. 
- **Template Grouping**: Now you can retrieve groups of templates based on customizable indexes. 
- **Median Time to Complete**: To display the estimated time required to complete the interview, based on data from thousands of Rocket Lawyer and partner customers over the past years. This helps end users understand how long it will take to complete the interview.
- **Short and Long Descriptions**: To provide clear document template descriptions to inform end users and ensure they have a solid understanding.
- **Thumbnails and Previews**: To enhance the user experience, visual thumbnails and previews are presented on interfaces.

## Resume Interviews
The resume interview feature is available for partners using the Embedded UX solution or creating custom UIs using the Rocket Lawyer API. It enables partners to use Rocket Lawyer to store half-completed interviews and resume them at a later time.

Partners using the Embedded UX solution can resume an interview by setting `pageId = "last"` within the `<rocket-document>` tag. Access the [RocketDocument Embedded UX](link) for additional information.

Partners creating their UI can use the [Get Interview by ID](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/get) endpoint to enable end users to resume their interviews. If a user exits the interview process, this endpoint allows partners to retrieve the interview details, including previously answered questions, and pick up where the user left off. For additional information, access [How to Build Your Own UI](/rocket-document-v2-build-your-own-ui).

# Further Information
You can find more information on upgrading RocketDocument Embedded UX from v1 to v2 using the [Guide: Upgrading to RocketDocument v2](/upgrading_to_rocketdocument_v2). You can also test the new v2 API through our [v2 Postman collection](link) and see the detailed [RocketDocument v2 API Reference](/docs/rocketdoc-api-product-sandbox/1/overview).

