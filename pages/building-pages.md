When you create your own UX, you are responsible for presenting all interview questions to the end user. This page covers the details of how to manage the interview questions.

# The Interview Process

Before describing how to manage the questions, it's important to have an overview of the interview process. Below is a summary of the necessary steps:

1. **Create an interview** ([Create an Interview](endpoint link)): When an interview is created, the `answersPayload` is initialized and included in the response. This `answersPayload` contains the default values for the document template and serves as the starting point for the interview process.
2. **Retrieve a Page in the Interview** ([Get Interview by Id](endpoint link)): This step will be different depending on the interview type:
    - **Ephemeral interviews**: The endpoint request will always return the default `answersPayload` associated with the document template.
    - **Persistent interviews**: The endpoint will return the `answersPayload` from the last saved point, allowing the interview to resume from where it was left off.
3. **Update an Interview Page** ([Update an Interview Page's Answers](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/patch)): When the end user answers a question, the partner should submit the updated `answersPayload` to Rocket Lawyer. The server processes this request, builds the next page based on the updated data, and returns the `answersPayload` along with the page data to the partner. 

> It is important to notice that you can use the [Get First Page](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/get) endpoint to retrieve the information related to the first interview question. In addition, if you want to resume an interview, you should use the [Get Page by ID](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/get) endpoint, which will return the information from the last answered question by the end user.

For a complete tutorial on how to create your own UI, access the [RocketDocument v2 Build your own UX ](https://rl-cicdv2-apigee-public-rocketdocv2.apigee.io/rocketdocument-v2-build-your-own-ux) page.

## Question Navigation

To navigate through the interview question, you will use: 

- [Get Page by ID](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/get): If you only need to retrieve question information, use this endpoint informing the ID of the page you want to retrieve.
- [Update an Interview Page's Answers](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/patch): If you have information from the end user to update, you can use this endpoint to update the interview progress and also receive information from the next, previous, or current question, based on the request configuration.

# Reponse Format

When you use one of the endpoints listed in the previous section, you will receive responses that follow a standard organization. The main response components are listed and described in the following table:

| **Parameter**   | **Description**                                                                                                       |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------|
| `name`                  | The name of the document template.                                                                                     |
| `answersPayload`        | Data structure to keep track of user-entered data.                                           |
| `previousPageData`      | Used for building UI elements and navigating through the interview.                          |
| `currentPageData`       | Used for building UI elements and navigating through the interview.                          |
| `nextPageData`          | Used for building UI elements and navigating through the interview.                          |
| `preview`               | Contains the HTML preview of the interview, including user input up to this point in the interview.  |

The parameters `previousPageData`, `currentPageData`, and `nextPageData` are objects composed of `format`, `pageId`, and `pageData`. Depending on the `format` type, each page will contain different data:

- `format = reference`: The page data will contain only a `pageId` and will be used to navigate the interview.
- `format = display`: The page data will contain all the information needed to render the page UI. Only one of `previousPageData`, `currentPageData`, or `nextPageData` will include the display format in a response. If multiple display formats are requested, only one will be returned, with preference given to `nextPageData`, then `currentPageData`.

The following code block presents an example of a response:

```json
{
  "name": "Lorem Ipsum Quill Document",
  "answersPayload": {
 <answersPayload>
 },
  "previousPageData": {
    "pageId": "<pageId>",
    "format": "reference"
 },
  "currentPageData": {
    "pageId": "<pageId>",
    "format": "display",
 <rest of pageData display>
 },
  "nextPageData": {
    "pageId": "<pageId>",
    "format": "reference"
 },
  "preview": {
    "format": null,
    "data": null
 }
}

```

In addition to the `pageId` and `format`, `previousPageData`, `currentPageData`, and `nextPageData` can have additional content depending on the API request.

## Page Data

When you request a page's content or update its content, the available information for one of the `previousPageData`, `currentPageData`, or `nextPageData` will include the data described in the following table.

| **Parameter**          | **Description**                                                                                                                                                                                   | **Type**      |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| `pageId`            | A unique identifier for the page. This is the primary key that uniquely identifies each page within the interview, essential for retrieving or referencing specific pages.                         | String        |
| `format`            | Indicates the format of the `pageData` object. It can be `reference` (used for navigation without detailed information) or `display` (containing all details necessary to render the UI).    | String        |
| `type`              | Specifies whether the page is `single` or `cyclical`. Single pages are standard pages, while cyclical pages repeat questions, which is useful for scenarios like listing multiple beneficiaries.           | String        |
| `cycleId`           | A unique identifier for the cycle within a cyclical page. This is present only if `type` is `cyclical` and ensures repeated questions are correctly associated with the right data.   | String        |
| `title`             | The title of the page is used primarily with chain cyclical. Optional for other pages, it provides context or a heading to a specific page.                                                         | String        |
| `progressPercentage`| The calculated interview percentage completed based on the current page. This optional field visually indicates how much of the interview has been completed.                   | String        |
| `questions`         | An array of question objects to be displayed on the page. Each question includes its own `id`, `title`, `fields`, visibility conditions, and other attributes necessary for UI rendering.         | Array         |
| `answers`           | An object or array of objects that contain user-provided answers to the questions. Cyclical pages will be an array since each cycle may contain multiple responses. For single, it will be an object.                       | Object/Array  |

Note that some parameters will only be available if `type=cyclical`. The following code block presents an example of a page data structure:

```json
{
  "pageId": "<opaque UUID of the page>",
  "format": "display",
  "type": "<single or cyclical>",
  "cycleId": "<opaque UUID of the cycle - present only if type=cyclical>",
  "title": "<optional for cyclical, used primarily with chain cyclicals>",
  "progressPercentage": "<calculated percentage of interview complete>",
  "questions": [
    "<array of question objects>"
 ],
  "answers": {
    "<answer object - this form for pageData type:single>"
 },
  "answers": [
    "<array of answer objects - this form for pageData type:cyclical>"
 ]
}

```

All parameters available in `questions` objects are listed and described in the following table:

| **Parameter** | **Description**                                                                                                       | **Type**  |
|---------------|-----------------------------------------------------------------------------------------------------------------------|-----------|
| `id`          | A unique identifier (UUID) for the question. This is used to reference the question within the page.                  | String    |
| `title`       | The title or main text of the question, which will be displayed to the user.                                          | String    |
| `hint`        | A short hint or guidance for the user, providing additional context for filling out the questions' associated fields. | String    |
| `help`        | Detailed help text that can be used in a "learn more" section, offering deeper explanations or instructions.          | String    |
| `fields`      | An array of field objects defining the question's input areas. Each field object includes parameters such as `id`, `label`, `type`, and `default`. | Array     |


### Questions Fields

Each field type tells the UI developer what kind of content to expect, helping them choose the right input method and set up proper validation on the user’s side. These field types don’t control how data is checked on the server. The following table lists and describes all parameter available for each field:

| **Parameter** | **Description**                                                                                                   |
|---------------|-------------------------------------------------------------------------------------------------------------------|
| `id`          | A unique identifier (UUID) for the field. This is used to reference the field within the page and in `answersPayload`. |
| `label`       | The label or name of the field that is displayed to the user.                                                   |
| `type`        | The field type, indicating what kind of input it accepts.                 |
| `default`     | The field's initial value before any user input.              |

> All field values, including true/false values, are stored as strings to keep consistency. The default value shows the field’s starting setting and includes no user-entered information.

Rocket Lawyer accepts several field types. The complete list is presented in the following table.

| **Field Type**   | **Description**                                                                                                                                              | **Example Use Case**              |
|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| `TEXT`           | A field designed for a single line of text. Commonly used for names, addresses, and other short text inputs.                                                 | "First Name", "Address"       |
| `PARAGRAPH`      | Similar to `TEXT` but allows for multiple lines or paragraphs. Useful for longer text inputs, such as property descriptions or comments.                     | "Property Description"          |
| `NUMBER`         | A field that should contain a numeric value. Suitable for inputs like age, quantity, or any other numerical data.                                             | "Age", "Quantity"             |
| `CURRENCY`       | A field with currency values, including a symbol denoting the currency type. When informing the currency, you can inform the desired symbol to use by adding the `symbol` parameter in addition to the `id`, `label`, `type`, and `default`. The template's country and language determine the correct symbol.   | "Price", "Amount"             |
| `PERCENTAGE`     | A field representing a percentage value. For example, 100 = 100%, 25 = 25%, etc.                                                                              | "Discount Rate", "Interest"   |
| `SSN`            | A specialized field for Social Security Numbers.                                                                                                             | "Social Security Number"        |
| `PHONE_NUMBER`   | A field for entering a phone number.                                                                                                                          | "Contact Number"                |
| `PHONE_EXT`      | A field for entering a phone extension.                                                                                                                       | "Extension"                     |
| `DATE`           | A field for date input, typically implemented with a date picker. Client-side validation might be included for format accuracy.                               | "Date of Birth", "Event Date" |
| `CHECKBOX`       | A field representing a selectable item, typically used for yes/no or true/false options.                                                                   | "Agree to Terms"                |
| `DROPDOWN`       | A field that allows the user to select one item from a list of options. The options are provided as an array within the field configuration through the additional `options` parameter.                 | "State", "Country"            |
| `RADIO`          | A group of radio buttons where only one option can be selected at a time. The default value determines which option is selected by default. There should only be one radio button group per question.                 | "Gender", "Preferred Contact" |

The following code block presents an example of a question composed of radio buttons, a dropdown, and a currency field:

```json
"fields": [
 {
    "id": "<opaque UUID>",
    "label": "Radio Option A",
    "type": "RADIO",
    "default": "true"
 },
 {
    "id": "<opaque UUID>",
    "label": "Radio Option B",
    "type": "RADIO",
    "default": "false"
 },
 {
    "id": "<opaque UUID of the field>",
    "label": "<field label>",
    "type": "DROPDOWN",
    "options": [
      "Option 1",
      "Option 2",
      "Option 3",
      "Option 4",
      "Option 5"
 ],
    "default": "<value>"
 },
 {
    "id": "<opaque UUID of the field>",
    "label": "<field label>",
    "type": "CURRENCY",
    "symbol": "$",
    "default": "<value>"
 }
]
```

### Questions Visibility

Some questions should only be shown when certain conditions are met. To control this, the question will contain an additional parameter `showIf` in its configuration, as in the following example.

**Example:**
```json
"showIf": "('<fieldid> <operator> [<value>]')"
```

The `showIf` configuration can combine multiple conditions using logical operators like `AND` and `OR`.

**Example with Multiple Conditions:**
```json
"showIf": "(
 ('Fl079pi2lujhu3 != California')
 AND ('Fl079pi2lujhu3 != District of Columbia')
 AND ('Fl079pi2lujhy1 = false')
 AND ('Fl079pi2lujhy2 > 1000')
 OR ('Fl079pi2lujhu3 = Colorado')
 AND ('Fl979r7f2pl52w = true')
)"
```

All supported operators are listed below:

- `=` : Equals.
- `!=` : Not equals.
- `>` : Greater than (for numeric fields).
- `>=` : Greater than or equal to (for numeric fields).
- `<` : Less than (for numeric fields).
- `<=` : Less than or equal to (for numeric fields).

> For cyclical questions, these conditions apply separately within each cycle based on the answers provided in that cycle.

## Cyclical Questions

Cyclical questions allow the UI to repeat the same set of questions for multiple items. For example, in a Will and Testament interview, you might need to ask about each beneficiary and what they will receive. These questions need to be repeated for each beneficiary.

To create a cyclical page, set the `type` to `cyclical` and define a `cycleId`. This tells the UI to repeat the questions and fields for each cycle. The `pageData` object for a cyclical page differs from a single page in two key ways:

1. It includes a `cycleId` to track each cycle.
2. The `answers` are stored as an array, with each item in the array corresponding to a different cycle.

# Updating the answersPayload

As users navigate the interview, the UI must capture their inputs and update the corresponding fields in the `answersPayload` using the [Submit Page](endpoint link) endpoint. For a text field `Name` in the `pageData`:

```json
{
  "id": "Flfmu8ta21l9eh",
  "label": "Name",
  "type": "TEXT",
  "default": ""
}
```

If the user enters `Bob`, the `answersPayload` should be updated as follows when the page is submitted:

```json
"answersPayload": {
  "Flfmu8ta21l9eh": "Bob"
}
```

The `answersPayload` is updated similarly for cyclical pages but with an array for each cycle. For example, if the user enters `Jane` and `30` in the first cycle and `Frank` and `40` in the second cycle:

```json
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
```
