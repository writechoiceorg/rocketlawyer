# Guide to Building a Custom UI with RocketDocument API v2

This guide will help you integrate directly with our API to build your custom user experience (UX) using RocketDocument v2. We'll cover the following:

1. Introduction to API v2
2. Understanding Response Body Format
3. Utilizing PageData for UI Rendering
4. Integrating GET & PATCH Endpoints
5. Handling User Inputs and Payload Updates

## 1. Introduction to API v2

RocketDocument API v2 provides comprehensive support for creating and managing interview UI pages. By leveraging the pageData elements within the response bodies of GET & PATCH `/interviews/:id/pages/:id` endpoints, you can build and customize interview pages dynamically.

### Resources

- **RocketDocument v2**: OpenAPI 3.1 specification.
- **General Page Flow**: Documentation on page navigation.
- **Paging Example**: Sample API response for pagination.

Note: Reference pages will be linked when published on RocketLawyer’s developer portal.

## 2. Understanding Response Body Format

Each response from the API contains six top-level elements essential for constructing the UI:

- `name`: The name of the document template.
- `answersPayload`: Tracks user-entered data.
- `previousPageData`, `currentPageData`, and `nextPageData`: Used for building UI elements and navigating through the interview.
- `preview`: Contains the HTML preview of the interview, including user input up to this point (not covered in this document).

### Example Response

```json
{
  "name": "Lorem Ipsum Quill Document",
  "answersPayload": { /* details below */ },
  "previousPageData": { "pageId": "<pageId>", "format": "reference" },
  "currentPageData": { "pageId": "<pageId>", "format": "display" },
  "nextPageData": { "pageId": "<pageId>", "format": "reference" },
  "preview": { "format": null, "data": null }
}
```

## 3. Utilizing PageData for UI Rendering

### Format Types

PageData can be in two formats: `reference` and `display`.

- **Reference**: Contains only `pageId` and is used for navigation.
- **Display**: Contains all necessary information for UI rendering.

### Display Format Example

```json
{
  "pageId": "<opaque UUID of the page>",
  "format": "display",
  "type": "<single or cyclical>",
  "cycleId": "<opaque UUID of the cycle (if cyclical)>",
  "progressPercentage": "<percentage of interview complete>",
  "questions": [ /* array of questions */ ],
  "answers": { /* answers object for single */ } or [ /* answers array for cyclical */ ]
}
```

### Questions and Fields

Each question includes:

```json
{
  "id": "<opaque UUID>",
  "title": "<question title>",
  "hint": "<hint text>",
  "fields": [ /* array of fields */ ]
}
```

Fields describe the UI elements, influencing the widgets to use and client-side validation.

### Field Types

- `TEXT`: Single line of text
- `PARAGRAPH`: Multiple lines or paragraphs
- `NUMBER`: Numeric values
- `CURRENCY`: Currency values
- `PERCENTAGE`: Percent values
- `SSN`: Social Security Number
- `PHONE_NUMBER`, `PHONE_EXT`: Phone numbers
- `DATE`: Date chooser
- `CHECKBOX`: Selectable items
- `DROPDOWN`: Single selection from a list
- `RADIO`: Single radio button group

#### Example of Field Types

```json
{
  "id": "Fl0ch40o9y0bgv",
  "label": "Name",
  "type": "TEXT",
  "value": "",
  "default": ""
}
```

### Visibility Conditions

Some questions should not be visible until a condition is met. If so, a question will contain:

```json
{
  "showIf": "condition(<fieldid> <operator> [<value>])"
}
```

Operators for visibility conditions include:

- `=`: field’s value is equal to value
- `!=`: field’s value is not equal to value
- `>`: greater than (number fields)
- `>=`: greater than or equal to (number fields)
- `<`: less than (number fields)
- `<=`: less than or equal to (number fields)

For cyclical questions, visibility determinations should be made locally to each cycle based on the answers within that cycle.

### Cyclicals

Cyclicals repeat questions for entities like beneficiaries in a Will. The `type` is set to `cyclical`, and `cycleId` is included.

#### Cyclical Example

```json
{
  "pageId": "<opaque UUID of the page>",
  "format": "display",
  "type": "cyclical",
  "cycleId": "Clfqvxpiwq5btl",
  "progressPercentage": "50",
  "questions": [ /* array of questions */ ],
  "answers": [
    {
      /* answers object */
    }
  ]
}
```

## 4. Integrating GET & PATCH Endpoints

### GET /interviews/:id/pages/:id

Use this endpoint to retrieve page data for rendering. The `currentPageData` always contains the display type pageData.

### PATCH /interviews/:id/pages/:id

This endpoint is used to update pageData and submit user responses. Ensure the answersPayload is updated and submitted along with the request.

#### Example PATCH Request

```json
{
  "currentPageData": { "format": "display" },
  "previousPageData": { "format": "reference" },
  "nextPageData": { "format": "reference" }
}
```

## 5. Handling User Inputs and Payload Updates

### answersPayload

The `answersPayload` is crucial for storing user inputs. It should be updated dynamically as users progress through the interview.

#### Example

For a text field:

```json
{
  "id": "Flfmu8ta21l9eh",
  "label": "Name",
  "type": "TEXT",
  "value": "",
  "default": ""
}
```

When the user enters "Bob":

```json
{
  "answersPayload": {
    "Flfmu8ta21l9eh": "Bob",
    // other fields
  }
}
```

### Cyclical Fields

For cyclical fields, update the `answersPayload` to reflect multiple cycles of user input.

#### Example

For two cycles with answers "Jane", "30" and "Frank", "40":

```json
{
  "Clfqvxpiwq5btl": [
    {
      "Flfqvvhivulxtb": "Jane",
      "Flfqvwpr3a35lw": "30"
    },
    {
      "Flfqvvhivulxtb": "Frank",
      "Flfqvwpr3a35lw": "40"
    }
  ]
}
```

### General PageData Construction Example

This is an example taken from Rocket Lawyer's Non-Disclosure Agreement:

```json
{
  "name": "Non-Disclosure Agreement US",
  "answersPayload": {
    "Fl0ccyw9hrz12w": "",
    "Fl0ce6o5rqiow6": "false",
    "Fl0cedn4nzqul0": "true",
    "Fl0ceh1ajbf26x": "",
    "Fl0cekmuq6lnn9": "",
    "Fl0cem469ome4s": "",
    "Fl0cenxq87uftg": "",
    "Fl0cf3mup66h8q": "",
    "Fl0cfafi2si05k": "",
    "Fl0cfe369zsoxq": "",
    "Fl0i6dpwfapy4d": "",
    "Fl0i6mcudwvkpb": "",
    "Fl0i6n12ngd2ef": "",
    "Fl0i6ocex3s5tq": "",
    "Fl0i717os3n6u6": "",
    "Fl0ch27vteziyw": "true",
    "Fl0ch34xos5g8a": "false",
    "Fl0ch40o9y0bgv": "",
    "Fl0ch98o15wo4f": "",
    "Fl0ch9ti8559j1": "",
    "Fl0cha8p851rgx": "",
    "Fl0chnmtifkffv": "",
    "Fl0choh96ekr71": "",
    "Fl0cisfa7duax7": "",
    "Fl0i4w8bwhjiox": "",
    "Fl0i55drikdxro": "",
    "Fl0i56likftmur": "",
    "Fl0i5c1dx7ypr2": "",
    "Fl0i5ugj10ck7f": "",
    "Fl0gtd9uxjpp3k": "",
    "Fl0cmyd6lm6g8f": "true",
    "Fl0cmzsv4l7jzy": "false",
    "Fl0cn1uret1n09": "true",
    "Fl0cn2i4feh7zz": "false",
    "Fl0cn75ajg3ecw": "",
    "Fl0cn9dxru9957": "",
    "version": 3
  },
  "previousPageData": {
    "pageId":

 "Ql0cfx84j81kd9",
    "format": "reference"
  },
  "currentPageData": {
    "pageId": "Fkbbc110f2d2eb",
    "type": "single",
    "format": "display",
    "progressPercentage": "78",
    "questions": [
      {
        "id": "Fkrp9n844hj7gtr",
        "label": "Who will receive the Confidential Information?",
        "hint": "Enter the name and address of the person who will be receiving the Confidential Information (the \"Recipient\"). Example: \"John Smith\".",
        "fields": [
          {
            "id": "Fl0ch40o9y0bgv",
            "label": "Name",
            "type": "TEXT",
            "value": "",
            "default": ""
          },
          {
            "id": "Fl0ch98o15wo4f",
            "label": "Address",
            "type": "TEXT",
            "value": "",
            "default": ""
          },
          {
            "id": "Fl0ch9ti8559j1",
            "label": "City",
            "type": "TEXT",
            "value": "",
            "default": ""
          },
          {
            "id": "Fl0cha8p851rgx",
            "label": "State",
            "type": "DROPDOWN",
            "value": "",
            "default": "",
            "options": [
              "Alabama",
              "Alaska",
              "Arizona",
              "Arkansas",
              "California",
              "Colorado",
              "Connecticut",
              "Delaware",
              "District of Columbia",
              "Florida",
              "Georgia",
              "Hawaii",
              "Idaho",
              "Illinois",
              "Indiana",
              "Iowa",
              "Kansas",
              "Kentucky",
              "Louisiana",
              "Maine",
              "Maryland",
              "Massachusetts",
              "Michigan",
              "Minnesota",
              "Mississippi",
              "Missouri",
              "Montana",
              "Nebraska",
              "Nevada",
              "New Hampshire",
              "New Jersey",
              "New Mexico",
              "New York",
              "North Carolina",
              "North Dakota",
              "Ohio",
              "Oklahoma",
              "Oregon",
              "Pennsylvania",
              "Rhode Island",
              "South Carolina",
              "South Dakota",
              "Tennessee",
              "Texas",
              "Utah",
              "Vermont",
              "Virginia",
              "Washington",
              "West Virginia",
              "Wisconsin",
              "Wyoming"
            ]
          },
          {
            "id": "Fl0chnmtifkffv",
            "label": "ZIP Code",
            "type": "TEXT",
            "default": ""
          }
        ]
      }
    ],
    "answers": {}
  },
  "nextPageData": {
    "pageId": "Ql0cmum8nx7sx2",
    "format": "reference"
  },
  "preview": {
    "format": null,
    "data": null
  }
}
```

## Conclusion

By following this guide, you can effectively build and customize your UX using RocketDocument API v2. Ensure to dynamically update the `answersPayload` and handle PageData correctly to create a seamless user experience.

For further details and examples, refer to the full API specification and accompanying resources.
