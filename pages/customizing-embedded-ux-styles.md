This documentation page provides guidance on customizing the **RocketDoc Embedded UX** component's styles using CSS variables. By leveraging these variables, Rocket Lawyer partners can align the user interface with their brand's visual identity, creating a consistent and cohesive user experience.

## Supported CSS Properties

The **RocketDoc Embedded UX** component supports a range of CSS properties that can be customized. These properties control various aspects of the user interface, including colors, fonts, and spacing. Below is a list of customizable CSS variables along with their default values.

### Action Button Styles

The appearance of action buttons can be customized by adjusting the following CSS variables:

- **--rl-action-primary-default**: Defines the primary color for action buttons.
  - Default: `#D68021`
  - Properties:
    - `Tell me more`: font color
    - `Checkboxes`: fill for all of them
    - `RadioButtons`: fill for all of them
    - `Add Another`: font color and fill for the plus sign
    - `Input fields`: border color when focused
    - `Primary buttons`: background color
    - `Secondary buttons`: font and border color

- **--rl-action-primary-disabled**: Defines the color for disabled action buttons.
  - Default: `#EFCCA6`
  - Properties:
    - `Primary buttons`: background color when disabled
    - `Secondary buttons`: font and border color when disabled

- **--rl-action-primary-hover**: Defines the hover color for action buttons.
  - Default: `#BA6102`
  - Properties:
    - `Primary buttons`: background color when hovering
    - `Secondary buttons`: border and font color when hovering

- **--rl-action-primary-pressed**: Defines the pressed color for action buttons.
  - Default: `#E89338`
  - Properties:
    - `Tell me more link`: changes the font color
    - `Primary buttons`: background color when active
    - `Add Another link`: changes the font color

### Attribution Styles

The following variables allow you to customize the "Powered by" attribution text displayed in the footer:

- **--rl-attribution-font-family**: Sets the font family that will be used for the `Powered By` attribution (if present) at the end of the page.

- **--rl-attribution-line-height**: Sets the line height used for the `Powered By` attribution (if present) at the end of the page.

### Banner Styles

Customize the appearance of banners by adjusting the following CSS variables:

- **--rl-banner-margin**: Defines the margin around the banner.  
- **--rl-banner-font-size**: Sets the font size for banner text.
- **--rl-banner-error-background-color**: Sets the background color for error banners.
- **--rl-banner-text-color**: Sets the text color in banners.

### Body Text Styles

Adjust the body text styles using the following CSS variables:

- **--rl-body-default-font-size**: Sets the font size for default body text.
- **--rl-body-default-font-weight**: Defines the font weight for default body text.
- **--rl-body-default-line-height**: Specifies the line height for default body text.
- **--rl-body-medium-font-size**: Sets the font size for medium body text.
- **--rl-body-medium-line-height**: Specifies the line height for medium body text.

### Button Styles

The appearance of buttons can be customized using the following CSS variables:

- **--rl-button-font-family**: Sets the font family for buttons.
- **--rl-button-border-radius**: Defines the border radius for buttons.
- **--rl-button-border-secondary**: Sets the border color for secondary buttons.

### Font and Label Styles

To customize fonts and labels, use the following CSS variables:

- **--rl-font-family**: Sets the default font family for the entire page.
- **--rl-heading-tertiary-font-size**: Specifies the font size for tertiary headings.
- **--rl-heading-tertiary-font-weight**: Defines the font weight for tertiary headings.
- **--rl-heading-tertiary-line-height**: Sets the line height for tertiary headings.
- **--rl-label-font-size**: Defines the font size for labels next to input fields.
- **--rl-label-font-weight**: Specifies the font weight for labels next to input fields.
- **--rl-label-letter-spacing**: Sets the letter spacing for labels next to input fields.
- **--rl-label-line-height**: Defines the line height for labels next to input fields.

### Input Field Styles

Customize input fields by modifying the following CSS variables:

- **--rl-input-font-size**: Sets the font size for input fields.
- **--rl-input-font-weight**: Defines the font weight for input fields.
- **--rl-input-padding**: Specifies the padding for input fields.
- **--rl-input-text**: Sets the text color for input fields.

### Progress Bar Styles

Adjust the appearance of the progress bar using the following CSS variables:

- **--rl-progress-bar-fill-color**: Sets the background color for the progress bar.
- **--rl-progress-bar-font-size**: Defines the font size for text inside the progress bar.
- **--rl-progress-bar-letter-spacing**: Specifies the letter spacing for text inside the progress bar.
- **--rl-progress-bar-line-height**: Sets the line height for text inside the progress bar.
- **--rl-progress-bar-percentage-font-weight**: Defines the font weight for the percentage text inside the progress bar.

### Additional Styles

Further customize the UI with these CSS variables:

- **--rl-powered-by**: Sets the font color for the "Powered by" attribution.
- **--rl-text-body-tertiary**: Sets the font color for tertiary body text.
- **--rl-text-header**: Defines the font color for headers.

## Implementing Custom Styles

To apply custom styles to the **RocketDoc Embedded UX** component, define the desired CSS variables in your stylesheet. These variables will override the default styles provided by the component.

### Example Implementation

```css
:root {
  --rl-action-primary-default: #FF5733;
  --rl-button-border-radius: 15px;
  --rl-heading-tertiary-font-size: 20px;
  --rl-label-font-weight: normal;
}
```

By defining custom styles in this manner, you can ensure that the **RocketDoc Embedded UX** component seamlessly integrates with your brand's design language, providing a consistent and polished user experience.