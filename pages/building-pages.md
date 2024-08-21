When you create your own UX, you are responsible for presenting all interview questions to the end user. This page covers the details of how to manage the interview questions.

# The Interview Process

Before describing how to manage the questions, it's important to have an overview of the interview process. Below is a summary of the necessary steps:

1. **Create an interview** ([Create an Interview](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/post)): When an interview is created, the `answersPayload` is initialized and included in the response. This `answersPayload` contains the default values for the document template and serves as the starting point for the interview process.
2. **Retrieve a Page in the Interview** ([Get Interview by Id](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/get)): This step will be different depending on the interview type:
   - **Ephemeral interviews**: The endpoint request will always return the default `answersPayload` associated with the document template.
   - **Persistent interviews**: The endpoint will return the `answersPayload` from the last saved point, allowing the interview to resume from where it was left off.
3. **Update an Interview Page** ([Update an Interview Page's Answers](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/patch)): When the end user answers a question, the partner should submit the updated `answersPayload` to Rocket Lawyer. The server processes this request, builds the next page based on the updated data, and returns the `answersPayload` along with the page data to the partner. 

> It is important to notice that you can use the [Get First Page](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/get) endpoint to retrieve the information related to the first interview question. In addition, if you want to resume an interview, you should use the [Get Page by ID](/docs/rocketdoc-api-product-sandbox/1/routes/interviews/%7BinterviewId%7D/pages/%7BpageId%7D/get) endpoint, which will return the information from the last answered question by the end user.

For a complete tutorial on how to create your own UI, access the [RocketDocument v2 Build Your Own UX](rocketdocument-v2-build-your-own-ux) page.

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

# Pages Examples

Rocket Lawyer provides the [Demo App](https://partner-demo-app.ip.cicdv2.sandbox.rocketlawyer.com/partners/logo-ipsum-2/document?templateId=0b0f7ca1-e59e-42cd-974d-a7bdc63645f6) to help you understand how to build interview pages. The following sections present the page data used to create each Demo App page.

## Page 1: Text Fields

The following image presents the first page content.


<img src="/files/demo-app-image1.png" alt="Text fields page" width=1024>


The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfmu8tao22p40",
  "format": "display",
  "type": "single",
  "progressPercentage": "0",
  "questions": [
    {
      "id": "Qlfmu8t9u32a73",
      "title": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
      "fields": [
        {
          "id": "Flfmu8ta21l9eh",
          "label": "For Shorter Answers",
          "type": "TEXT",
          "default": ""
        },
        {
          "id": "Flfmuapr3merom",
          "label": "For Longer Answers",
          "type": "PARAGRAPH",
          "default": ""
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 2: Other Fields

The following image presents the seccond page content.

<img src="/files/demo-app-image2.png" alt="Other fields page" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfmuc7bp91na3",
  "format": "display",
  "type": "single",
  "progressPercentage": "8",
  "questions": [
    {
      "id": "Qlfmuc7alkdhgw",
      "hint": "Vestibulum lectus mauris ultrices eros in. Sed lectus vestibulum mattis ullamcorper velit sed. Nunc sed id semper risus in hendrerit gravida rutrum. Dui accumsan sit amet nulla facilisi morbi tempus iaculis urna. Ac tortor dignissim convallis aenean et. Lacinia quis vel eros donec ac odio tempor orci dapibus.",
      "title": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
      "fields": [
        {
          "id": "Flfmuc7avnckno",
          "label": "For Specific Dates",
          "type": "DATE",
          "default": ""
        },
        {
          "id": "Flfmudkmyins28",
          "label": "For Numbers",
          "type": "NUMBER",
          "default": ""
        },
        {
          "id": "Flfmudy12wpt42",
          "label": "For Currency Fields",
          "type": "CURRENCY",
          "default": ""
        },
        {
          "id": "Flfmueaitww2si",
          "label": "For Percentages",
          "type": "PERCENTAGE",
          "default": ""
        },
        {
          "id": "Flfmueus054smy",
          "label": "For Social Security Numbers",
          "type": "SSN",
          "default": ""
        },
        {
          "id": "Flfmuf9d0rxhhy",
          "label": "For Phone Numbers",
          "type": "PHONE_NUMBER",
          "default": ""
        },
        {
          "id": "Flfmufliy5gnuf",
          "label": "For Phone Extensions",
          "type": "PHONE_EXT",
          "default": ""
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 3: Dropdowns

The following image presents the third page content.

<img src="/files/demo-app-image3.png" alt="Dropdowns Image" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfqvjc6l4o4u8",
  "format": "display",
  "type": "single",
  "progressPercentage": "17",
  "questions": [
    {
      "id": "Qlfqvjc49rfy5f",
      "title": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
      "fields": [
        {
          "id": "Flfqvjc5djla5m",
          "label": "For answers from a set number of options",
          "type": "DROPDOWN",
          "options": [
            "Option 1",
            "Option 2",
            "Option 3",
            "Option 4",
            "Option 5"
          ],
          "default": ""
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 4: Checkboxes

The following image presents the fourth page content.

<img src="/files/demo-app-image4.png" alt="Checkboxes image" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfqvit43sqo1v",
  "format": "display",
  "type": "single",
  "progressPercentage": "25",
  "questions": [
    {
      "id": "Qlfqvit2q2xfq0",
      "hint": "Vestibulum lectus mauris ultrices eros in. Sed lectus vestibulum mattis ullamcorper velit sed. Nunc sed id semper risus in hendrerit gravida rutrum. Dui accumsan sit amet nulla facilisi morbi tempus iaculis urna. Ac tortor dignissim convallis aenean et. Lacinia quis vel eros donec ac odio tempor orci dapibus.",
      "title": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
      "fields": [
        {
          "id": "Flfqvit30l6v1v",
          "label": "Checkbox Options 1",
          "type": "CHECKBOX",
          "default": "false"
        },
        {
          "id": "Flfqvnozj9u8r7",
          "label": "Checkbox Options 2",
          "type": "CHECKBOX",
          "default": "false"
        },
        {
          "id": "Flfqvnvr121l11",
          "label": "Checkbox Options 3",
          "type": "CHECKBOX",
          "default": "false"
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 5: Radio Buttons 

The following image presents the fifth page content.

<img src="/files/demo-app-image5.png" alt="Radio Buttons" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfmujgzikgt81",
  "format": "display",
  "type": "single",
  "progressPercentage": "33",
  "questions": [
    {
      "id": "Qlfmujgxlrwily",
      "hint": "",
      "title": "This is a question based on Radios. Select Option A to have the next question in sequence appear.",
      "fields": [
        {
          "id": "Flfmulpp4orak5",
          "label": "Radio Option A",
          "type": "RADIO",
          "default": "true"
        },
        {
          "id": "Flfmum2u1mcvif",
          "label": "Radio Option B",
          "type": "RADIO",
          "default": "false"
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 6: Conditionals

The following image presents the sixth page content. This page uses conditionals which are handled by the server side.

<img src="/files/demo-app-image6.png" alt="Conditionals" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfmuuwzedl04e",
  "format": "display",
  "type": "single",
  "progressPercentage": "42",
  "questions": [
    {
      "id": "Qlfmuuwx10sjly",
      "hint": "Select option D to have the next question appear.",
      "title": "This question will appear when Radio Option A is selected.",
      "fields": [
        {
          "id": "Flfmuuwxkfljt6",
          "label": "Radio Option C",
          "type": "RADIO",
          "default": "true"
        },
        {
          "id": "Flfmuwllpj97qz",
          "label": "Radio Option D",
          "type": "RADIO",
          "default": "false"
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 7: Conditionals 

The following image presents the seventh page content. This page uses conditionals which are handled by the server side.


<img src="/files/demo-app-image7.png" alt="Conditionals" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfmuxn81cb728",
  "format": "display",
  "type": "single",
  "progressPercentage": "50",
  "questions": [
    {
      "id": "Qlfmuxn4q5gzt4",
      "hint": "",
      "title": "This question will appear when Radio Option A and Radio Option D are BOTH selected.",
      "fields": [
        {
          "id": "Flfmuxn5g03q4z",
          "label": "Radio Option E",
          "type": "RADIO",
          "default": "true"
        },
        {
          "id": "Flfmuytjst5yo9",
          "label": "Radio Option F",
          "type": "RADIO",
          "default": "false"
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 8: Conditionals

The following image presents the eighth page content. This page uses conditionals which are handled by the server side.

<img src="/files/demo-app-image8.png" alt="Conditionals" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfmuznu0nlsb6",
  "format": "display",
  "type": "single",
  "progressPercentage": "58",
  "questions": [
    {
      "id": "Qlfmuznqy9h345",
      "title": "This question will appear if Radio Option A and Radio Option C are selected OR if Radio Option B and Radio Option D are selected.",
      "hint": "",
      "fields": [
        {
          "id": "Flfmuznrnxr8vn",
          "label": "Text Field",
          "type": "TEXT",
          "default": ""
        }
      ]
    }
  ],
  "answers": {
  }
}
```

## Page 9: Cyclicals

The following image presents the ninth page content regarding the first cyclical answer.


<img src="/files/demo-app-image9.png" alt="Cyclical first" width=1024>

The following image presents the ninth page content regarding the second cyclical answer.

<img src="/files/demo-app-image10.png" alt="Cyclical second" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfqvs6we4in99",
  "format": "display",
  "type": "cyclical",
  "cycleId": "Clfqvty59mpobo",
  "progressPercentage": "67",
  "questions": [
    {
      "id": "Qlfqvs6u935a34",
      "hint": "The user can input as many entries as needed. Notice how the punctuation adapts in the boiler plate according to the number of entries.",
      "title": "This is a simple cyclical question with one answer.",
      "fields": [
        {
          "id": "Flfqvs6uomr7a8",
          "label": "Cyclical Answer 1",
          "type": "TEXT",
          "default": ""
        }
      ]
    }
  ],
  "answers": [
  ]
}
```

## Page 10: Cyclical with Multiple Fields

The following image presents the tenth page content.

<img src="/files/demo-app-image11.png" alt="Cyclical with Multiple Fields" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfqvvhkwshzo1",
  "format": "display",
  "type": "cyclical",
  "cycleId": "Clfqvxpiwq5btl",
  "progressPercentage": "75",
  "questions": [
    {
      "id": "Qlfqvvhibiztva",
      "title": "This is a simple cyclical question with more than one answer.",
      "hint": "",
      "fields": [
        {
          "id": "Flfqvvhivulxtb",
          "label": "Cyclical Answer 2",
          "type": "TEXT",
          "default": ""
        },
        {
          "id": "Flfqvwpr3a35lw",
          "label": "Date",
          "type": "DATE",
          "default": ""
        }
      ]
    }
  ],
  "answers": [
  ]
}
```

## Page 11: Chain Question with Conditionals

The following image presents the eleventh page content when **Chain 3 Radio** is selected.

<img src="/files/demo-app-image12.png" alt="Chain 3 Radio" width=1024>

The following image presents the eleventh page content when **Chain 4 Radio** is selected.

<img src="/files/demo-app-image13.png" alt="Chain 4 Radio" width=1024>

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfqykq1ry32lk",
  "format": "display",
  "type": "single",
  "progressPercentage": "83",
  "questions": [
    {
      "id": "Qlfqykpwkmsmfx",
      "hint": "More than one question will appear on the same page, if logic dictates they should be grouped as a question.",
      "title": "This is a chain question.",
      "fields": [
        {
          "id": "Flfqykpxer721j",
          "label": "Date",
          "type": "DATE",
          "default": ""
        },
        {
          "id": "Flfqyoh037bgfr",
          "label": "Chain 2",
          "type": "TEXT",
          "default": ""
        }
      ]
    },
    {
      "id": "Qlfqykusjwfsqy",
      "hint": "To trigger the next chain question to appear on this page, select Chain 4 radio.",
      "title": "This is the second chain question.",
      "fields": [
        {
          "id": "Flfqyp3z2r6prc",
          "label": "Chain 3 Radio",
          "default": "true",
          "type": "RADIO",
          "default": ""
        },
        {
          "id": "Flfqypitjtpdvs",
          "label": "Chain 4 Radio",
          "default": "false",
          "type": "RADIO",
          "default": ""
        }
      ]
    },
    {
      "id": "Qlfqykymvr6c8m",
      "title": "This question will pop up if the Chain 3 Radio is selected.",
      "hint": "",
      "showIf": "condition(Flfqypitjtpdvs = true)",
      "fields": [
        {
          "id": "Flfqykynx3au7f",
          "label": "Dropdown Option of US States",
          "type": "DROPDOWN",
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
        }
      ]
    }
  ],
  "answers": {
  }
}

```

## Page 12: Complex Cyclicals

The following image presents the twelfth page content.

<img src="/files/demo-app-image14.png" alt="Complex Cyclical" width=1024>

Cyclical with Multiple Fields

The following code block presents the data used to build the above page.

```json
{
  "pageId": "Plfqwers2moy7d",
  "format": "display",
  "type": "cyclical",
  "cycleId": "Clfqxxxexkmjzr",
  "progressPercentage": "92",
  "questions": [
    {
      "id": "Qlfqwernnhnfnj",
      "hint": "This comprises of chain questions. Three individual questions which have been joined together on one page. The last question will pop up is the corresponding checkbox is triggered.",
      "title": "This is a complex cyclical question.",
      "fields": [
        {
          "id": "Flfqwerocs85au",
          "label": "Complex Cyclical 1",
          "type": "TEXT",
          "default": ""
        }
      ]
    },
    {
      "id": "Qlfqxqtkv06v5x",
      "hint": "Joining questions together creates a chain cyclical.",
      "title": "This is the second question on the same page, which makes it a chain cyclical.",
      "fields": [
        {
          "id": "Flfqxqtlgdisas",
          "label": "Enter a Number",
          "type": "NUMBER",
          "default": ""
        },
        {
          "id": "Flfqxr4tv21nvc",
          "label": "Check the box to trigger the next question to appear",
          "type": "CHECKBOX",
          "default": ""
        }
      ]
    },
    {
      "id": "Qlfqxs3m3vujui",
      "hint": "This is the third question in the chain cyclical.",
      "title": "This chain cyclical question will pop up if the checkbox is ticked.",
      "showIf": "condition(Flfqxr4tv21nvc = true)",
      "fields": [
        {
          "id": "Flfqxyudep8lza",
          "label": "Complex cyclical 4",
          "type": "TEXT",
          "default": ""
        }
      ]
    }
  ],
  "answers": [
  ]
}

```