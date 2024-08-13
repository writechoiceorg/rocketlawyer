## Package Overview

The main purpose is to have a single JavaScript file that partners can include on their sites to run an interview.

### Hosts

- **Sandbox:** `https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js`
- **Production:** `https://rocket-document.rocketlawyer.com/rocket-document.js`

### Description

This is an embeddable web component that partners can use to run an interview without the need to code everything themselves. The benefits include:

- A UI that shows every page with all the questions and fields that a user has to answer to get the final document.
- Events fired by the component so the parent UI can react to each one.
- All the logic is implemented to navigate from one page to the next.

### Target Audience

Partners

### Author

- **Ulises Estecche**

### Reviewers

- Chris Hodapp
- Erik Altman
- Ashok Subramanian
- Brion Moss
- Abhijeet Vijayakar
- Sumit Malhotra
- Amit Bagree (Deactivated)
- Mark Walker (Deactivated)

## Glossary

- **ParentUI:** The web application that embeds the RocketDoc embeddable UI, usually developed by a Rocket Lawyer partner.
- **RocketDocEUI:** The RocketDoc embeddable UI.
- **RLBE:** Refers to any RocketLawyer backend service.

## Package Overview

The main purpose is to have a single JavaScript file that partners can include on their sites to run an interview.

## Component Details

### `<rocket-document>` Component

This is the only component that this package officially supports.

### Attributes

| Attribute       | Description                                                                                                                                                                |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `serviceToken`  | This is the access or service token that delegates access to perform actions over a specific interview. It is required to have brand information so the UI can load the specifics for each partner. The RocketDocument API supports two service token purposes that may be used here: `api.rocketlawyer.com/binder-party-access` for partners using RocketSign, and `api.rocketlawyer.com/rocketdoc` for partners using RocketDocument. The token includes properties to show the UI in a personalized way based on each partner's choices, such as styles and available elements. |
| `accessToken`   | Can be used instead of `serviceToken`. This should be a scoped access token specific to this interview. See the row above for the scoped access information.                                                     |
| `interviewId`   | This is the interview UUID needed so the RocketDocEUI can show all the questions and fields.                                                                                 |
| `pageId`        | An optional attribute; the purpose is to allow the caller to indicate an interview page that the RocketDocEUI will open. Options include: `first` (opens the first page of the interview), `last` (opens the last updated page of the interview), and `$pageId` (a UUID page ID of a page in an interview). Note that this is not an incrementing value, does not imply order of interview pages, and cannot be guessed or predicted. |

## Custom Events

These are custom events fired by the RocketDocument component to the embedding UI. These events are not network-bound or data-centric.

### Event List

| Event                   | Description                                                                                                                                                    |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `interview-loading`      | This is the first event that the RocketDocEUI fires. It is fired just before the call to the RLBE to get the interview.                                         |
| `interview-started`      | Deprecated in favor of `interview-loaded` but retained for backward compatibility. Indicates that the RocketDocEUI got a response from the RLBE and is now rendering the interview in the background. |
| `interview-loaded`       | The second event that the RocketDocEUI fires, signifying that the interview is being rendered.                                                                  |
| `loading-next-page`      | Fired when the user clicks the "Continue" button, indicating the RocketDocEUI is aware of the user asking to go to the next page and before calling the RLBE.  |
| `loading-previous-page`  | Fired when the user clicks the "Back" button, indicating the RocketDocEUI is aware of the user asking to go to the previous page and before calling the RLBE. |
| `question-answered`      | Fired every 10 seconds if answers have changed to prevent data loss if the user abandons the interview.                                                        |
| `page-changed`           | Fired after a successful response from the RLBE, confirming that answers were saved. Also fired at the last question of the interview.                         |
| `interview-completing`   | Indicates that the interview completion process has started.                                                                                                   |
| `interview-completed`    | The last event fired by the RocketDocEUI, indicating the interview is complete and no further processes are running.                                           |
| `rocketdocumenterror`    | Deprecated but retained for backward compatibility. New event is `interview-error`.                                                                            |
| `interview-error`        | Fired when something bad happens during certain actions, especially when calling the RLBE.                                                                     |
| `component-connected`    | Related to lifecycle method `connectedCallback()` which is called by Stencil when connected to the DOM.                                                       |
| `component-disconnected` | Related to lifecycle method `disconnectedCallback()` which is called by Stencil when disconnected from the DOM.                                               |

## Listening to an Event

Since the component fires several events, the ParentUI should listen for any of those. To accomplish that, the ParentUI needs to add a JavaScript code section to the page where the RocketDocEUI was embedded. For example:

```javascript
<script>
  document
    .querySelector("rocket-document")
    .addEventListener("interview-loading", (e) => {
      console.log("interview-loading event received", e.detail);
    });
</script>
```

## Globals

### CSS Variables

All the CSS variables available are documented on the [RocketDoc Embedded UI stylesheet spec](https://enterprise.resources.sandbox.rocketlawyer.com/groups/8802d520-da9f-4c48-995c-395017315cd1/configs/brand).

CSS variables can be sourced by the partner in two ways:
1. Through partner-level branding loaded from enterprise resources.
2. Overriding CSS values by using/reading a partner CSS.

## Discussions, Decisions, and Action Items from the Council Review

- The `pageNumber` parameter inside events has been renamed to `pageId` for consistency across the RocketDoc V2 platform.
- Add scoping of tokens to the interview itself for Rocket Lawyer's access, speeding up page load times.
- Add the ability for partners to modify the configuration via attributes.
- Document that the component will not submit events to Rocket Lawyer servers to track activity.
