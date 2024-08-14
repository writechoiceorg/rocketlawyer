This documentation page provides guidance on customizing the **RocketDoc Embedded UX** component's styles using CSS variables. By leveraging these variables, Rocket Lawyer partners can align the user interface with their brand's visual identity, creating a consistent and cohesive user experience.

## Supported CSS Properties

The **RocketDoc Embedded UX** component supports a range of CSS properties that can be customized. These properties control various aspects of the user interface, including colors, fonts, and spacing. Below is a list of customizable CSS variables along with their default values.

### Action Button Styles

The appearance of action buttons can be customized by adjusting the following CSS variables:

- **--rl-action-primary-default**: Defines the primary color for action buttons.
  - Default: `#1473E6`
- **--rl-action-primary-disabled**: Defines the color for disabled action buttons.
  - Default: `#B3B3B3`
- **--rl-action-primary-hover**: Defines the hover color for action buttons.
  - Default: `#2A67C9`
- **--rl-action-primary-pressed**: Defines the pressed color for action buttons.
  - Default: `#095ABA`

### Attribution Styles

The following variables allow you to customize the "Powered by" attribution text displayed in the footer:

- **--rl-attribution-font-family**: Sets the font family for the attribution text.
  - Default: `Arial`
- **--rl-attribution-line-height**: Sets the line height for the attribution text.
  - Default: `24px`

### Banner Styles

Customize the appearance of banners by adjusting the following CSS variables:

- **--rl-banner-close-background-image**: Specifies the background image for the banner close button.
- **--rl-banner-error-icon-background-image**: Specifies the background image for the error icon in the banner.
- **--rl-banner-margin**: Defines the margin around the banner.
  - Default: `0px 16px 0px 16px`
- **--rl-banner-font-size**: Sets the font size for banner text.
  - Default: `13px`
- **--rl-banner-error-background-color**: Sets the background color for error banners.
  - Default: `#C9252D`
- **--rl-banner-text-color**: Sets the text color in banners.
  - Default: `white`

### Body Text Styles

Adjust the body text styles using the following CSS variables:

- **--rl-body-default-font-size**: Sets the font size for default body text.
  - Default: `13px`
- **--rl-body-default-font-weight**: Defines the font weight for default body text.
  - Default: `regular`
- **--rl-body-default-line-height**: Specifies the line height for default body text.
  - Default: `24px`
- **--rl-body-medium-font-size**: Sets the font size for medium body text.
  - Default: `14px`
- **--rl-body-medium-line-height**: Specifies the line height for medium body text.
  - Default: `19px`

### Button Styles

The appearance of buttons can be customized using the following CSS variables:

- **--rl-button-font-family**: Sets the font family for buttons.
  - Default: `'Noto Sans', sans-serif`
- **--rl-button-border-radius**: Defines the border radius for buttons.
  - Default: `25px`
- **--rl-button-border-secondary**: Sets the border color for secondary buttons.
  - Default: `#1473E6`

### Font and Label Styles

To customize fonts and labels, use the following CSS variables:

- **--rl-font-family**: Sets the default font family for the entire page.
  - Default: `'Times New Roman', serif`
- **--rl-heading-tertiary-font-size**: Specifies the font size for tertiary headings.
  - Default: `22px`
- **--rl-heading-tertiary-font-weight**: Defines the font weight for tertiary headings.
  - Default: `bold`
- **--rl-heading-tertiary-line-height**: Sets the line height for tertiary headings.
  - Default: `30px`
- **--rl-label-font-size**: Defines the font size for labels next to input fields.
  - Default: `13px`
- **--rl-label-font-weight**: Specifies the font weight for labels next to input fields.
  - Default: `bold`
- **--rl-label-letter-spacing**: Sets the letter spacing for labels next to input fields.
  - Default: `0em`
- **--rl-label-line-height**: Defines the line height for labels next to input fields.
  - Default: `24px`

### Input Field Styles

Customize input fields by modifying the following CSS variables:

- **--rl-input-font-size**: Sets the font size for input fields.
  - Default: `13px`
- **--rl-input-font-weight**: Defines the font weight for input fields.
  - Default: `400`
- **--rl-input-padding**: Specifies the padding for input fields.
  - Default: `0.7143rem`
- **--rl-input-text**: Sets the text color for input fields.
  - Default: `#000000`

### Progress Bar Styles

Adjust the appearance of the progress bar using the following CSS variables:

- **--rl-progress-bar-fill-color**: Sets the background color for the progress bar.
  - Default: `#E7F2FE`
- **--rl-progress-bar-font-size**: Defines the font size for text inside the progress bar.
  - Default: `11px`
- **--rl-progress-bar-letter-spacing**: Specifies the letter spacing for text inside the progress bar.
  - Default: `0px`
- **--rl-progress-bar-line-height**: Sets the line height for text inside the progress bar.
  - Default: `13px`
- **--rl-progress-bar-percentage-font-weight**: Defines the font weight for the percentage text inside the progress bar.
  - Default: `bold`

### Additional Styles

Further customize the UI with these CSS variables:

- **--rl-powered-by**: Sets the font color for the "Powered by" attribution.
  - Default: `#9EA0AC`
- **--rl-text-body-tertiary**: Sets the font color for tertiary body text.
  - Default: `#9DA0AC`
- **--rl-text-header**: Defines the font color for headers.
  - Default: `#000000`

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