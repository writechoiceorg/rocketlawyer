This documentation page provides guidance on customizing the **RocketDocument Embedded UX** component's styles using CSS variables. By leveraging these variables, Rocket Lawyer partners can align the user interface with their brand's visual identity, creating a consistent and cohesive user experience.

## Customizing the Embedded UX Component

To tailor the **RocketDocument Embedded UX** Component to match your brand's look and feel, follow the steps below:

1. **Submit Your Style Preferences**:
   - Fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSddJPPg0onclKYf2IIyRehCYwlTtlcogXXXxo0ZlwtZLd3ZZQ/viewform?fbzx=5836941555539130795) with your desired styles, including colors, logos, and button corner radius.

2. **Create a Request in the Partner Portal**:
   - Once your style preferences are ready, submit a customization request through the [partner portal](https://rocket-lawyer.atlassian.net/servicedesk/customer/portal/10).

These steps will ensure that the **RocketDocument Embedded UX** reflects your brand’s identity and provides a consistent user experience across your platform.

## Supported CSS Properties

The **RocketDocument Embedded UX** component supports a range of CSS properties that can be customized. These properties control various aspects of the user interface, including colors, fonts, and spacing. Below is a list of customizable CSS variables along with their default values.

### Interview Form Styles

The following variables allow you to customize the color and appearance of action buttons and related interactive elements within the interview form:

- `--rl-action-primary-default`: Sets the primary color for interactive elements within the interview form. The default color is `#D68021`, and it is applied to the following components:

  - **Tell me more link**: Text font color
  - **Checkboxes**: Fill color for all checkboxes
  - **RadioButtons**: Fill color for all radio buttons
  - **Add Another**: Font color and plus sign color
  - **Input fields**: Border color when focused
  - **Primary buttons**: Background color
  - **Secondary buttons**: Font and border color

- `--rl-action-primary-disabled`: Sets the color for action buttons when they are disabled. The default color is `#EFCCA6`, and it is applied to the following components:

  - **Primary buttons**: Background color when disabled
  - **Secondary buttons**: Font and border color when disabled

- `--rl-action-primary-hover`: Defines the color for action buttons when hovered over. The default color is `#BA6102`, and it is applied to the following components:

  - **Primary buttons**: Background color when hovering
  - **Secondary buttons**: Border and font color when hovering

- `--rl-action-primary-pressed`: Sets the primary color for interactive elements when they are pressed or active. The default color is `#E89338`, and it is applied to the following components:

  - **Tell me more link**: Changes the font color
  - **Primary buttons**: Background color when active
  - **Add Another link**: Changes the font color

### Attribution Styles

The following variables allow you to customize the `Powered by` attribution text displayed in the footer:

- `--rl-attribution-font-family`: Sets the font family that will be used for the `Powered By` attribution (if present) at the end of the page.

- `--rl-attribution-line-height`: Sets the line height used for the `Powered By` attribution (if present) at the end of the page.

### Banner Styles

Customize the appearance of banners by adjusting the following CSS variables:

- `--rl-banner-margin`: Defines the margin values for the error banner.
- `--rl-banner-font-size`: Defines the font size for the text in the error banner.
- `--rl-banner-error-background-color`: Defines the background color for the error banner.
- `--rl-banner-text-color`: Defines the font color for the text in the error banner.

### Body Text Styles

Adjust the body text styles using the following CSS variables:

- `--rl-body-default-font-size`: Defines the font size for the text that shows up below the interview question.
- `--rl-body-default-font-weight`: Defines the font weight for the text that shows up below the interview question.
- `--rl-body-default-line-height`: Defines the line height used for the text that shows up below the interview question (usually defined as the question hint).
- `--rl-body-medium-font-size`: Defines the font size of the document title present above the progress bar.
- `--rl-body-medium-line-height`: Defines the line height of the text above the progress bar, including the document title and the `Saved` text that appears at some point.

### Button Styles

The appearance of buttons can be customized using the following CSS variables:

- `--rl-button-font-family`: Defines the font family for all the buttons present in the interview. For example, `Continue`, `Back`, `Preview document`, etc.
- `--rl-button-border-radius`: Defines the border radius for all the buttons present in the interview.
- `--rl-button-border-secondary`: Defines the border color for all the secondary buttons like the `Back` and `Continue answering` buttons.

### Font and Label Styles

To customize fonts and labels, use the following CSS variables:

- `--rl-font-family`: Defines the font family used as the default one for the whole page unless a different one is defined on some specific sections.
- `--rl-heading-tertiary-font-size`: Defines the font size for the text of each question.
- `--rl-heading-tertiary-font-weight`: Defines the font weight for the text of each question.
- `--rl-heading-tertiary-line-height`: Defines the line height for the text of each question.
- `--rl-label-font-size`: Defines the font size for all the input fields like `dropdowns`, `textarea`, and `number` fields. 
- `--rl-label-font-weight`: Defines the font weight for all the input fields like `dropdowns`, `textarea`, and `number` fields.
- `--rl-label-letter-spacing`: Defines the letter spacing for all the labels next to the input fields, and also it applies to the checkboxes and radio buttons texts.
- `--rl-label-line-height`: Defines the line height for all the labels next to the input fields.

### Input Field Styles

Customize input fields by modifying the following CSS variables:

- `--rl-input-font-size`: Defines the font size for all the input fields like `dropdowns`, `textarea`, and `number` fields.
- `--rl-input-font-weight`: Defines the font weight for all the input fields like `number`, `textarea` and `date` fields.
- `--rl-input-padding`: Defines the padding for all the input fields like `number`, `textarea` and `date` fields.
- `--rl-input-text`: defines the font color for all the input fields like `dropdowns`, `textarea` and `date` fields.

### Progress Bar Styles

Adjust the appearance of the progress bar using the following CSS variables:

- `--rl-progress-bar-fill-color`: Defines the background color for the progress bar.
- `--rl-progress-bar-font-size`: Defines the font size of the text inside the progress bar.
- `--rl-progress-bar-letter-spacing`: Defines the letter spacing of the text inside the progress bar.
- `--rl-progress-bar-line-height`: Defines the line height of the text inside the progress bar.
- `--rl-progress-bar-percentage-font-weight`: Defines the font weight of the text that shows the overall percentage inside the progress bar; it does not include the text `PROGRESS`.

### Additional Styles

Further customize the UI with these CSS variables:

- `--rl-powered-by`: Defines the font color for the `Powered by` attribution at the end of the page.
- `--rl-text-body-tertiary`: Defines the font color for the `Saved` text that shows up when the interview is saved.
- `--rl-text-header`: This defines the font color for the following elements:
  - Question title: `What payment methods will be accepted?`
  - Question description (hint): `If you'd like to add another form of payment, enter it in the space provided below`
  - Label for all the fields: `Other Form of Payments`


