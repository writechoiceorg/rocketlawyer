This page outlines the key changes in RocketDoc API v2, highlighting new features such as custom user experience options, enhanced data handling with Data I/O, and updates to embedded user experiences. It also details adjustments to API endpoints, additional response information, and improvements in event handling, providing a comprehensive overview for users familiar with v1 who are exploring the upgrades in v2.

## New Features

### Custom User Experience

API partners can now create a [custom user experience directly through the API](pages/rocket-document-v2-build-your-own-ui). This feature allows full customization, enabling partners to seamlessly reflect their brand identity and visual preferences within the user experience. It offers the same utilities as the [RocketDocument Embedded UX](/pages/glossary), such as choosing the [template](/pages/glossary), persistent and ephemeral [interviews](/pages/glossary), and generating the finished [document](/pages/glossary), but with more customization options.

### Data I/O

The [Data I/O feature](pages/data-i-o-addon-feature-integration) allows partners to import data to and extract data from interviews using [Tagged Answer Models (TAM)](/pages/glossary). TAMs are the model of questions that a particular template uses.

#### Benefits

- **Structured Data Retrieval:** Retrieve the Tagged Answer Model (TAM) for any interview, enabling seamless data integration by mapping customer data directly.
- **Pre-filled Interviews:** Improve user experience by pre-filling interview fields with existing customer data, saving time and reducing redundancy.
- **Data Accessibility:** Easily access and retrieve user-provided data to further customize and enhance the document creation process.

## Additional Enhancements

### Embedded User Experience
We have completely rebuilt RocketDocument Embbeded UX to work with the new v2 endpoints. However, the user experience remains largely similar for embedded users. Some adjustments were made to improve it, which you can see below:
- **Adjustments to Endpoints:** Endpoints have changed from the v1 format (`/rocketdoc/v1/interviews`) to the v2 format (`/v2/interviews`). Check the [Upgrading to RocketDocument v2](pages/upgrading_to_rocketdocument_v2) page for more information.
- **More Information on Responses:** The v2 endpoints also return more information, such as the new `medianMinutesToComplete` object in the [Retrieve a Template by ID](link) endpoint that returns how many minutes an end user takes to complete an interview of that template.
- **Changes to Events:** The list of events was updated. Check the [UX Component Events](pages/ux-component-events) page for more information.

### Templates
Easily retrieve a list of templates a partner can access through the [Get Templates List](link) endpoint. With the change to v2 endpoints, partners can now get HTML previews of documents, PDF thumbnails of a document's first page, short or long document descriptions, and the median time to complete an interview.

### Customized Document List
Each partner can request a set of documents to be enabled for them, granting a customized set of templates. Partners can, for example, identify and filter documents specific to certain states, ensuring compliance and relevance.

### Resume Interviews
Partners can use the [Retrieve Interview by ID](link) endpoint to allow end users to resume their interviews from where they left off.

## Further Information

You can find more information on how to adapt your v1 setup for the new v2 endpoints in the [Quick Start Guide: Upgrading to RocketDocument v2](/upgrading_to_rocketdocument_v2). You can also test the new v2 API through our [v2 Postman collection](link).
