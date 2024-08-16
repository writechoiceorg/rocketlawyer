This page outlines the key changes in RocketDoc API v2, highlighting new features such as custom user experience options, enhanced data handling with Data I/O, and updates to embedded user experiences. It also details adjustments to API endpoints, additional response information, and improvements in event handling, providing a comprehensive overview for users familiar with v1 who are exploring the upgrades in v2.

## New Features

### Custom User Experience

API partners can now create a [custom user experience directly through the API](pages/rocket-document-v2-build-your-own-ui). This feature allows full customization, enabling partners to seamlessly reflect their brand identity and visual preferences within the user experience. The RocketDoc API replaces the [RocketDocument Embedded UX](/pages/glossary), offering additional features:

- You can choose the [template](/pages/glossary) to use on the interview.
In addition to persistent interviews, you can use ephemeral [interviews](/pages/glossary). When using ephemeral interviews, no partner's data is persisted on Rocket Lawyer servers.
- You can customize the interview journey, retrieving what questions to ask and document previews from API endpoints.  

### Data I/O

The [Data I/O feature](pages/data-i-o-addon-feature-integration) allows partners to import data to and extract data from interviews using [Tagged Answer Models (TAM)](/pages/glossary). TAMs represent answers and the format required to complete a particular  [template](/pages/glossary). 

#### Benefits

- **Structured Data Retrieval:** Access the Tagged Answer Model (TAM) for any interview, allowing you to view the questions presented to the client, input pre-existing answers, or retrieve data from previously completed interviews.
- **Pre-filled Interviews:** Improve user experience by pre-filling interview fields with existing customer data, saving time and reducing redundancy.
- **Data Accessibility**: Easily access and retrieve user-provided data, enabling re-usage for new interviews and enhancing the similar document creation process.

## Additional Enhancements

### Embedded User Experience
We have completely rebuilt RocketDocument Embedded UX to work with the new v2 endpoints. However, the user experience remains largely similar for embedded users. Some adjustments were made to improve it, which you can see below:
- **Adjustments to Endpoints:** Endpoints have changed from the v1 format (`/rocketdoc/v1/interviews`) to the v2 format (`/v2/interviews`). Check the [Upgrading to RocketDocument v2](pages/upgrading_to_rocketdocument_v2) page for more information.
- **Changes to Events:** The list of events was updated. For more information, check the [UX Component Events](pages/ux-component-events) page.

### Templates
Retrieve a customized list of templates available to each partner through the [Get Templates List](link). With the v2 endpoint enhancements, partners can access:

- HTML previews of documents.
- PDF thumbnails of the first page.
- Detailed short or long descriptions of each template.

Partners can also request specific document sets tailored to their needs, filtering by state or other criteria to ensure compliance and relevance.

## Interviews

- **Resume Interviews**: Partners can use the [Retrieve Interview by ID](link) endpoint to allow end users to resume their interviews from where they left off.
- **Median Time to Complete**: Display the estimated time required to complete the interview, based on data from thousands of Rocket Lawyer and partner customers over the past years. This helps end users understand how long it will take to complete the interview.
- **Short and Long Descriptions**: Provide clear document template descriptions to inform end users and ensure they have a solid understanding.
- **Thumbnails and Previews**: Enhance the user experience by presenting visual thumbnails and previews on interfaces.

## Further Information

You can find more information on how to adapt your v1 setup for the new v2 endpoints in the [Quick Start Guide: Upgrading to RocketDocument v2](/upgrading_to_rocketdocument_v2). You can also test the new v2 API through our [v2 Postman collection](link).
