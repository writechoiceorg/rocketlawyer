This guide outlines how to customize the RocketDoc Embedded UX component's styles using CSS variables. These variables provide flexibility for branding and user experience, allowing Rocket Lawyer partners to tailor the UI to match their brand's look and feel.

## Supported CSS Properties

The RocketDoc Embedded UX component supports a variety of CSS properties that can be customized. These properties control different aspects of the UI, including colors, fonts, and spacing. Below is a list of customizable CSS variables and their default values:

### Action Button Styles
- **--rl-action-primary-default**: Defines the primary color for buttons.
  - Default: `#1473E6`
- **--rl-action-primary-disabled**: Defines the color for disabled buttons.
  - Default: `#B3B3B3`
- **--rl-action-primary-hover**: Defines the hover color for buttons.
  - Default: `#2A67C9`
- **--rl-action-primary-pressed**: Defines the pressed color for buttons.
  - Default: `#095ABA`

### Attribution Styles
- **--rl-attribution-font-family**: Sets the font family for the "Powered by" attribution.
  - Default: `Arial`
- **--rl-attribution-line-height**: Sets the line height for the "Powered by" attribution.
  - Default: `24px`

### Banner Styles
- **--rl-banner-close-background-image**: Background image for the banner close button.
- **--rl-banner-error-icon-background-image**: Background image for the error icon in the banner.
- **--rl-banner-margin**: Sets the margin for the banner.
  - Default: `0px 16px 0px 16px`
- **--rl-banner-font-size**: Sets the font size for text in the banner.
  - Default: `13px`
- **--rl-banner-error-background-color**: Background color for the error banner.
  - Default: `#C9252D`
- **--rl-banner-text-color**: Font color for the text in the banner.
  - Default: `white`

### Body Text Styles
- **--rl-body-default-font-size**: Font size for default body text.
  - Default: `13px`
- **--rl-body-default-font-weight**: Font weight for default body text.
  - Default: `regular`
- **--rl-body-default-line-height**: Line height for default body text.
  - Default: `24px`
- **--rl-body-medium-font-size**: Font size for medium body text.
  - Default: `14px`
- **--rl-body-medium-line-height**: Line height for medium body text.
  - Default: `19px`

### Button Styles
- **--rl-button-font-family**: Font family for buttons.
  - Default: `'Noto Sans', sans-serif`
- **--rl-button-border-radius**: Border radius for buttons.
  - Default: `25px`
- **--rl-button-border-secondary**: Border color for secondary buttons.
  - Default: `#1473E6`

### Font and Label Styles
- **--rl-font-family**: Default font family for the entire page.
  - Default: `'Times New Roman', serif`
- **--rl-heading-tertiary-font-size**: Font size for tertiary headings.
  - Default: `22px`
- **--rl-heading-tertiary-font-weight**: Font weight for tertiary headings.
  - Default: `bold`
- **--rl-heading-tertiary-line-height**: Line height for tertiary headings.
  - Default: `30px`
- **--rl-label-font-size**: Font size for labels next to input fields.
  - Default: `13px`
- **--rl-label-font-weight**: Font weight for labels next to input fields.
  - Default: `bold`
- **--rl-label-letter-spacing**: Letter spacing for labels next to input fields.
  - Default: `0em`
- **--rl-label-line-height**: Line height for labels next to input fields.
  - Default: `24px`

### Input Field Styles
- **--rl-input-font-size**: Font size for input fields.
  - Default: `13px`
- **--rl-input-font-weight**: Font weight for input fields.
  - Default: `400`
- **--rl-input-padding**: Padding for input fields.
  - Default: `0.7143rem`
- **--rl-input-text**: Font color for input fields.
  - Default: `#000000`

### Progress Bar Styles
- **--rl-progress-bar-fill-color**: Background color for the progress bar.
  - Default: `#E7F2FE`
- **--rl-progress-bar-font-size**: Font size for text inside the progress bar.
  - Default: `11px`
- **--rl-progress-bar-letter-spacing**: Letter spacing for text inside the progress bar.
  - Default: `0px`
- **--rl-progress-bar-line-height**: Line height for text inside the progress bar.
  - Default: `13px`
- **--rl-progress-bar-percentage-font-weight**: Font weight for the percentage text inside the progress bar.
  - Default: `bold`

### Additional Styles
- **--rl-powered-by**: Font color for the "Powered by" attribution.
  - Default: `#9EA0AC`
- **--rl-text-body-tertiary**: Font color for tertiary body text.
  - Default: `#9DA0AC`
- **--rl-text-header**: Font color for headers.
  - Default: `#000000`

## Implementing Custom Styles

To customize the RocketDoc Embedded UX component, define the desired CSS variables in your stylesheet. These variables will override the default styles applied by the component.

### Example
```css
:root {
  --rl-action-primary-default: #FF5733;
  --rl-button-border-radius: 15px;
  --rl-heading-tertiary-font-size: 20px;
  --rl-label-font-weight: normal;
}
```

By applying these custom styles, you can ensure that the RocketDoc Embedded UX component aligns with your brand's visual identity while maintaining a consistent user experience.

## Conclusion

Customizing the RocketDoc Embedded UX component's styles is a straightforward process that enables partners to create a branded, cohesive user experience. Utilize the CSS variables listed in this document to tailor the component's appearance to your specific needs.