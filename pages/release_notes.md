RocketDoc API v2 brings significant improvements and new features to enhance the experience for both API partners and end users. This document provides an overview of the key updates, focusing on enhancements in the embedded user experience, new functionalities like Data I/O for pre-filling and retrieving interview data, and additional features designed to streamline and optimize the document creation process.

## New Features

### Custom User Experience

API partners can now create a [custom user experience directly through the API](pages/build_custom_ui.md). This feature allows full customization, enabling partners to reflect their brand identity and visual preferences seamlessly within the user experience.

### Data I/O

The [Data I/O feature](pages/data-i-o-addon-feature-integration) allows partners to import data to and extract data from interviews using Tagged Answer Models (TAM).

#### Benefits

- **Structured Data Retrieval:** Retrieve the Tagged Answer Model (TAM) for any interview, enabling seamless data integration by mapping customer data directly.
- **Pre-filled Interviews:** Improve user experience by pre-filling interview fields with existing customer data, saving time and reducing redundancy.
- **Data Accessibility:** Easily access and retrieve user-provided data to further customize and enhance the document creation process.

## Additional Enhancements

- **Embedded User Experience:** The embedded user experience remains largely similar to the previous version, with only minor [adjustments to endpoints](pages/upgrading_to_rocketdocument_v2.md) and [events](pages/ux-component-events.md).
- **List Templates:** Easily retrieve a list of templates through the [Get Templates List](link) endpoint.
- **Median Time to Complete:** Access data regarding the median time to complete each template, helping partners optimize the interview process.
- **State-Specific Documents:** Partners can now identify and filter documents specific to certain states, ensuring compliance and relevance.
- **Resume Interviews:** By using the [Retrieve Interview by ID](link) endpoint, partners can allow end users to resume their interviews from where they left off.