This guide is designed to help Rocket Lawyer partners easily integrate the RocketDoc interview process into their web applications. This guide provides detailed instructions on configuring the component, handling custom events during the interview process, and customizing the UI with CSS. By following this documentation, developers can seamlessly embed and manage the RocketDoc UI, ensuring a smooth user experience while aligning with their brand's look and feel.

## Package Overview

The RocketDoc package provides a streamlined solution for integrating the RocketDoc interview process into partner websites with minimal effort. By including a single JavaScript file, partners can easily embed the RocketDoc UI component into their web pages, allowing users to seamlessly conduct interviews.

Here’s how to implement the integration:

```javascript
  <script src="https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js">
  </script>
```

Once the JavaScript file is included, simply add the RocketDocEUI tag within the HTML body like this:

```javascript
  <rocket-document
    serviceToken={rl-rdoc-servicetoken}
    interviewId={interview-id}
    pageId={page-ref}>
  </rocket-document>
```

### Package Name

- `rocket-document.js`

### Hosts

- **Sandbox:** `https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js`
- **Production:** `https://rocket-document.rocketlawyer.com/rocket-document.js`

### Description

This is an embeddable web component that partners can use to run an interview without the need to actually code everything themselves. The benefits are as follows:
- A UI that shows every page with all the questions and fields that a user has to answer to get the final document.
- Events being fired by the component so the parent UI can react to each one.
- All the logic is implemented to go from one page to the next one.

## Sequence Diagram

Once the page loads, the `RocketDocEUI` component retrieves the interview associated with the specified ID by calling the RLBE service. It then displays the first page of the interview, including all relevant questions and fields. Note that `InterviewIds` are auto-generated UUIDs, which are neither incremental nor predictable. The following diagram illustrates this process:

![Sequence Diagram](/media/start-interview.png)

### **Event Sequence**

This section outlines the key events in the sequence diagram that occur when loading the `RocketDocEUI` component and displaying the first interview page. Understanding these events is essential for effective integration and error handling, ensuring a smooth user experience.

1. **Load Component (ID)**
   - **Action:** The `ParentUI` initiates the process by loading the `RocketDocEUI` component. This is done by embedding the component on the webpage, usually identified by a unique ID.
   - **Purpose:** This step starts the `RocketDocEUI` component, preparing it to interact with the Rocket Lawyer Backend (RLBE) to fetch the necessary interview data.

2. **interview-loading**
   - **Action:** Once the `RocketDocEUI` component is loaded, it immediately fires the `interview-loading` event.
   - **Purpose:** This event informs the `ParentUI` that the interview is in the process of being loaded. It’s a signal to possibly show a loading indicator or perform other preparatory actions in the UI.

3. **Load Partner Specific Configuration**
   - **Action:** The `RocketDocEUI` requests partner-specific configurations from the RLBE.
   - **Purpose:** This ensures that the UI loads with all the necessary customizations and branding specific to the partner implementing the interview.

4. **Load Brand/Rocket-Doc Information**
   - **Action:** The RLBE responds with the requested partner-specific configurations and branding information.
   - **Purpose:** This data is essential for personalizing the `RocketDocEUI` according to the partner's requirements.

5. **Get Interview (ID)**
   - **Action:** The `RocketDocEUI` makes a request to the RLBE to fetch the interview data associated with the provided ID.
   - **Purpose:** This step retrieves the content and structure of the interview that needs to be displayed to the user.

6. **Interview Info (Found)**
   - **Action:** If the interview with the specified ID is found, the RLBE returns the interview information.
   - **Purpose:** This data includes all the questions and fields required for the interview, which will be displayed to the user.

7. **Interview Info (Received)**
   - **Action:** The `RocketDocEUI` receives the interview information from the RLBE.
   - **Purpose:** This marks the successful retrieval of the interview data, and the component is ready to render the first page of the interview.

8. **interview-loaded**
   - **Action:** The `RocketDocEUI` fires the `interview-loaded` event.
   - **Purpose:** This event informs the `ParentUI` that the interview has been successfully loaded and is ready to be displayed. The UI can now proceed to show the first page of the interview to the user.

9. **Interview Not Found**
   - **Action:** If the interview with the specified ID is not found, the RLBE returns an error indicating that the interview was not found.
   - **Purpose:** This handles cases where the interview ID provided is incorrect or the interview has been deleted.

10. **rocket-document-error**
    - **Action:** The `RocketDocEUI` fires the `rocket-document-error` event.
    - **Purpose:** This event informs the `ParentUI` that an error occurred while attempting to load the interview. The UI can then display an appropriate error message to the user, indicating that the interview could not be found.

## Component Details

### `<rocket-document>` Component

This is the only component that this package officially supports.

### Attributes

| Attribute       | Description                                                                                                                                                                  |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `serviceToken`  | This is the access or service token that delegates access to perform actions over a specific interview. It is required to have brand information so the UI can load specifics for each partner. The RocketDocument API supports two service token purposes that may be used here: `api.rocketlawyer.com/binder-party-access` for partners using RocketSign, and `api.rocketlawyer.com/rocketdoc` for partners using RocketDocument. |
| `accessToken`   | Can be used instead of `serviceToken`. This should be a scoped access token specific to this interview. See the row above for the scoped access information.                                                      |
| `interviewId`   | This is the interview UUID needed so the `RocketDocEUI` can show all the questions and fields.                                                                                   |
| `pageId`        | An optional attribute; allows the caller to indicate an interview page that the `RocketDocEUI` will open. Options: `first`, `last`, or `$pageId` (a UUID page id of a page in an interview). |

## Custom Events

These are custom events fired by the RocketDocument component to the embedding UI. These events are not Partner Events or US Frontend event handler events. They are not network-bound or data-centric.

### Event List

#### Event: interview-loading

- **Name:** `interview-loading`
- **State:** Existing
- **CustomEvent Schema:**
    
     ```javascript
       type: "interview-loading"
     ```

- **Description:** This is the first event that the `RocketDocEUI` fires. It is fired just before the call to the RLBE to get the interview.

#### Event: interview-started

- **Name:** `interview-started`
- **State:** Deprecated
- **CustomEvent Schema:**
    
     ```javascript
       type: "interview-started"
       detail: {
         "interviewId": "<interview uuid>",
         "deprecated": true
       }
     ```
     
- **Description:** This is the second event fired. It has been deprecated in favor of the more appropriately named `interview-loaded` but is retained for backward compatibility. The event name does not accurately reflect what it signifies because the backend is actually starting the event. It is also confusing because of the partner event `INTERVIEW_STARTED`.

#### Event: interview-loaded

- **Name:** `interview-loaded`
- **State:** Updated
- **CustomEvent Schema:**
    
     ```javascript
       type: "interview-loaded"
       detail: {
         "interviewId": "<interview uuid>",
         "pageId": "<page uuid>"
       }
     ```
     
- **Description:** This is the second event that the `RocketDocEUI` fires. This means that the `RocketDocEUI` got a response from the RLBE, received a valid interview from the API, and is now rendering the interview in the background.

#### Event: loading-next-page

- **Name:** `loading-next-page`
- **State:** New
- **CustomEvent Schema:**
    
     ```javascript
        type: "loading-next-page"
        detail: {
          "pageId": "<next page uuid>"
        }
     ```
     
- **Description:** This event is fired by the `RocketDocEUI` once the user clicks the "Continue" button. This tells the ParentUI that the `RocketDocEUI` is aware of the user asking to go to the next page and before calling the RLBE.

#### Event: loading-previous-page

- **Name:** `loading-previous-page`
- **State:** New
- **CustomEvent Schema:**
    
     ```javascript
        type: "loading-previous-page"
          detail: {
          "pageId": "<previous page uuid>"
        }
     ```

- **Description:** This event is fired by the `RocketDocEUI` once the user clicks the "Back" button. This tells the ParentUI that the `RocketDocEUI` is aware of the user asking to go to the previous page and before calling the RLBE.

#### Event: question-answered

- **Name:** `question-answered`
- **State:** Updated
- **CustomEvent Schema:**
    
     ```javascript
        type: "question-answered"
        detail: {
          "pageId": "<page uuid>",
          "pageTitle": "<page title when available>"
        }
     ```

- **Description:** This event is fired every 10 seconds if changes in answers are detected, ensuring nothing is lost in case the user abandons the interview.

#### Event: page-changed

- **Name:** `page-changed`
- **State:** New
- **CustomEvent Schema:**
    
     ```javascript
        type: "page-changed"
        detail: {
          "pageId": "<page uuid>",
          "pageTitle": "<page title when available>"
        }
     ```

- **Description:** This event is fired by the `RocketDocEUI` once it receives a successful response from the RLBE, meaning that all answers were actually saved. This is useful if the `RocketDocEUI` is going to show a success message. It is also fired at the last question of the interview since the `RocketDocEUI` will receive a response from the RLBE indicating that all answers were saved.

#### Event: interview-completing

- **Name:** `interview-completing`
- **State:** Existing
- **CustomEvent Schema:**
    
     ```javascript
        type: "interview-completing"
     ```

- **Description:** This event indicates that the completion of the interview has started. At this point, the user is at the end of the interview, and the "Continue" button is disabled since there are no more questions to answer.

#### Event: interview-completed

- **Name:** `interview-completed`
- **State:** Existing
- **CustomEvent Schema:**
    
     ```javascript
        type: "interview-completed"
        detail: {
          "binderId": "<binder uuid>"
        }
     ```

- **Description:** This event is the last event fired by the `RocketDocEUI`. It means that the interview is completed and filled with all the info provided by the user. It will also tell the ParentUI that no further processes are running or yet remain to be completed. It is up to the ParentUI to take the user to the next screen since the `RocketDocEUI` only fires the event.

#### Event: rocketdocumenterror

- **Name:** `rocketdocumenterror`
- **State:** Deprecated
- **CustomEvent Schema:**
    
     ```javascript
        type: "interview-error"
        detail: {
          "code": "<code>",
          "message": "<string with the error msg>",
          "deprecated": true
        }
     ```

- **Description:** Deprecated but retained for backward compatibility. The new event is `interview-error`. This event is fired when something bad happens while performing certain actions, especially when calling the RLBE.

#### Event: interview-error

- **Name:** `interview-error`
- **State:** New
- **CustomEvent Schema:**
    
     ```javascript
        type: "interview-error"
        detail: {
          "code": "<code>",
          "message": "<string with the error msg>"
        }
     ```

> **Possible Error Codes:**
> 
>   `<code>` can be one of the following values:
>   - COMPLETION_CONNECTION_ERROR
>   - COMPLETION_ERROR
>   - LOAD_INTERVIEW_ERROR
>   - LOAD_QUESTION_ERROR
>   - LOAD_QUESTION_CONNECTION_ERROR
>   - SAVE_ERROR

- **Description:** This event is fired when something bad happens while performing certain actions, especially when calling the RLBE.

#### Event: component-connected

- **Name:** `component-connected`
- **State:** New
- **CustomEvent Schema:**
    
     ```javascript
        type: "component-connected"
     ```

- **Description:** Related to the lifecycle method `connectedCallback()` which will be implemented by this component and called by Stencil when connected to the DOM.

#### Event: component-disconnected

- **Name:** `component-disconnected`
- **State:** New
- **CustomEvent Schema:**
    
     ```javascript
        type: "component-disconnected"
     ```

- **Description:** Related to the lifecycle method `disconnectedCallback()` which will be implemented by this component and called by Stencil when disconnected from the DOM.

> **Deprecated Events Notice:**
> 
> - The `interview-started` event has been deprecated in favor of the `interview-loaded` event. 
> - The `rocketdocumenterror` event has been deprecated in favor of `interview-error` to better describe the nature of errors.

                 
## Listening to an Event

Since the component fires several events, the ParentUI should listen for any of those. To accomplish that, the ParentUI needs to add a JavaScript code section to the page in which the `RocketDocEUI` was embedded. It will be something like the following:

```javascript
<script>
  document
    .querySelector("rocket-document")
    .addEventListener("interview-loading", (e) => {
      console.log("interview-loading event received", e.detail);
    });
</script>
```
## Glossary

This glossary defines key terms used throughout this guide to help you better understand the concepts and components involved.

| Term          | Description                                                                                                              |
|---------------|--------------------------------------------------------------------------------------------------------------------------|
| **ParentUI**  | The web application that embeds the RocketDoc embeddable UI, usually an application developed by a Rocket Lawyer partner. |
| **RocketDocEUI** | The RocketDoc embeddable UI.                                                                                            |
| **RLBE**      | Refers to any RocketLawyer backend service. 

## Globals

This section covers global settings and configurations that apply across the RocketDoc UI component. These settings allow partners to customize and control the behavior and appearance of the component to better align with their branding and user experience requirements.

### CSS Variables

The RocketDoc UI component supports a variety of CSS variables that allow for extensive customization of the interface's look and feel. Detailed documentation of all available CSS variables can be found in the [RocketDoc Embedded UI stylesheet spec](https://enterprise.resources.sandbox.rocketlawyer.com/groups/8802d520-da9f-4c48-995c-395017315cd1/configs/brand).

CSS variables can be sourced by the partner in two ways:

1. Through partner-level branding loaded from enterprise resources.
2. Overriding CSS values by using/reading a partner CSS.

## Discussions, Decisions, and Action Items from the Council Review

Here, we summarize key discussions and decisions made during the council review process regarding the RocketDoc V2 platform.

- The `pageNumber` parameter inside events has been renamed to `pageId` for consistency across the RocketDoc V2 platform.
- Add scoping of tokens to the interview itself for Rocket Lawyer's access, speeding up page load times.
- Add the ability for partners to modify the config via attributes.
- Document that the component will not submit events to Rocket Lawyer servers to track activity.
