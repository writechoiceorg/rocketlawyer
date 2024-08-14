This guide will help you to integrate the RocketDocument Embedded UX interview process into your web applications. This guide provides detailed instructions on configuring the component, handling custom events during the interview process, and customizing the UI with CSS. By following this documentation, developers can seamlessly embed and manage the RocketDocument UI, ensuring a smooth user experience while aligning with their brand's look and feel.

## Package Overview

The RocketDocument package provides a streamlined solution for integrating the RocketDocument interview process into partner websites with minimal effort. By including a single JavaScript file, partners can easily embed the RocketDocument UI component into their web pages, allowing users to conduct interviews seamlessly. The RocketDocument UI component provides:

- A UI that shows every page with all the questions and fields a user must answer to get the final document.
- The component is firing events so the parent UI can react to each one.
- An automatic logic that goes from one page to the next one.

The following code block describes how to execute the integration:

```html
  <script src="https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js">
  </script>
```

> **Available environments**
> The following environments are available when using the component:
>   - **Sandbox:** `https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js`
>   - **Production:** `https://rocket-document.rocketlawyer.com/rocket-document.js`

Once the JavaScript script is included, add the RocketDocument Embedded UX tag within the HTML body like displayed in the following code block:

```html
  <rocket-document
    serviceToken={rl-rdoc-servicetoken}
    interviewId={interview-id}
    pageId={page-ref}>
  </rocket-document>
```

## Sequence Diagram

Once the page loads, the RocketDocument Embedded UX component retrieves the interview associated with the specified ID by calling the RLBE service. RLBE refers to any RocketLawyer backend service. It then displays the first page of the interview, including all relevant questions and fields. Note that `InterviewIds` are auto-generated UUIDs, which are neither incremental nor predictable. The following diagram illustrates this process:

![Sequence Diagram](/media/start-interview.png)

### Event Sequence

Below is the outline of the key events in the sequence diagram that occur when loading the RocketDocument Embedded UX component and displaying the first interview page. Understanding these events is essential for effective integration and error handling, ensuring a smooth user experience.

1. **load component (id)**: The process begins when the `ParentUI` initiates the loading of the RocketDocument component, which is accomplished by embedding the component on the webpage. This step is required to activate the RocketDocument component, enabling it to interact with the Rocket Lawyer Backend (RLBE) to retrieve the necessary interview data.

2. **interview-loading**: Once the RocketDocument component is loaded, it immediately fires the `interview-loading` event. This event informs the `ParentUI` that the interview is in the process of being loaded, signaling that a loading indicator or other preparatory actions may need to be displayed in the UI.

3. **load Partner specific conf**: The RocketDocument component requests partner-specific configurations from the RLBE. This ensures that the UI is loaded with all necessary customizations and branding specific to the partner implementing the interview.

4. **brand/rocket-doc info**: The RLBE responds with the requested partner-specific configurations and branding information. This data is essential for personalizing the RocketDocument component according to the partner's requirements.

5. **getInterview (id)**: The RocketDocument component makes a request to the RLBE to fetch the interview data associated with the provided ID. This step retrieves the content and structure of the interview that needs to be displayed to the user.

6. **interview info (Found)**: If the interview with the specified ID is found, the RLBE returns the interview information. This data includes all the questions and fields required for the interview, which will be displayed to the user.

7. **interview info (Received)**: The RocketDocument component receives the interview information from the RLBE. This marks the successful retrieval of the interview data, and the component is now ready to render the first page of the interview.

8. **interview-loaded**: The RocketDocument component fires the `interview-loaded` event. This event informs the `ParentUI` that the interview has been successfully loaded and is ready to be displayed, allowing the UI to proceed with showing the user the first page of the interview.

9. **interview not found**: If the interview with the specified ID is not found, the RLBE returns an error indicating that the interview could not be located. This handles cases where the provided interview ID is incorrect, or the interview has been deleted.

10. **rocket-document-error**: The RocketDocument component fires the `rocket-document-error` event. This event informs the `ParentUI` that an error occurred while attempting to load the interview, prompting the UI to display an appropriate error message to the user, indicating that the interview could not be found.

## Component Details

Below, we provide an overview of the `<rocket-document>` component, detailing its attributes and how it integrates with partner applications.

### Attributes

The `<rocket-document>` component uses attributes that allow it to control how the interview is displayed and managed.

| Attribute       | Description                                                                                                                                                                  |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `serviceToken`  | This is the access or service token that delegates access to perform actions over a specific interview. Brand information is required so the UI can load specifics for each partner. The RocketDocument API supports two service token purposes that may be used here: `api.rocketlawyer.com/binder-party-access` for partners using RocketSign and `api.rocketlawyer.com/rocketdoc` for partners using RocketDocument. |
| `accessToken`   | Can be used instead of `serviceToken`. This should be a scoped access token specific to this interview. See the row above for the scoped access information.                                                      |
| `interviewId`   | This is the interview UUID needed so the RocketDocument Embedded UX can show all the questions and fields.                                                                                   |
| `pageId` | An optional attribute that allows the caller to indicate an interview page that the RocketDocument Embedded UX will open. Options: `first`, `last`, or `$pageId` (a UUID page id of a page in an interview). |

## Listening to an Event

Since the component fires several events, the `ParentUI` can listen for any of those. To listen to the events, a JavaScript code must be added to the page where the RocketDocument Embedded UX was embedded. The following code block presents an example that adds an event listener to the `rocket-document` component, which will print the details of every `interview-loading` event.

```html
<script>
  document
    .querySelector("rocket-document")
    .addEventListener("interview-loading", (e) => {
      console.log("interview-loading event received", e.detail);
    });
</script>
```

## Custom Events

These are custom events fired by the RocketDocument component to the embedding UI. These events are not Partner Events or US Frontend event handler events. They are not network-bound or data-centric. Next, you find a list of all possible events that can be fired.

### interview-loading

The `interview-loading` is the first event that the RocketDocument Embedded UX fires. It is fired just before the call to the RLBE to get the interview. Below, you can find the `interview-loading` schema: 
    
```javascript
   type: "interview-loading"
```

###  interview-started

The `interview-started` is the second event fired. It has been deprecated in favor of the more appropriately named `interview-loaded` but is retained for backward compatibility. The event name does not accurately reflect what it signifies because the backend is actually starting the event. It is also confusing because of the partner event `INTERVIEW_STARTED`. Below, you can find the `interview-started` schema: 
    
```javascript
   type: "interview-started"
   detail: {
     "interviewId": "<interview uuid>",
     "deprecated": true
   }
```
     
### interview-loaded

The `interview-loaded` is the second event that the RocketDocument Embedded UX fires. This means that the RocketDocument got a response from the RLBE, received a valid interview from the API, and is now rendering the interview in the background. Below, you can find the `interview-loaded` schema: 
    
```javascript
   type: "interview-loaded"
   detail: {
     "interviewId": "<interview uuid>",
     "pageId": "<page uuid>"
   }
```   

### loading-next-page

The `loading-next-page` event is fired by the RocketDocument Embedded UX once the user clicks the "Continue" button. This tells the `ParentUI` that the RocketDocument is aware of the user asking to go to the next page and before calling the RLBE. Below, you can find the `loading-next-paged` schema: 
    
```javascript
   type: "loading-next-page"
   detail: {
      "pageId": "<next page uuid>"
   }
```

### loading-previous-page

The `loading-previous-page` event is fired by the RocketDocument Embedded UX once the user clicks the "Back" button. This tells the `ParentUI` that the RocketDocument is aware of the user asking to go to the previous page and before calling the RLBE.  Below, you can find the `loading-previous-page` schema: 

```javascript
   type: "loading-previous-page"
      detail: {
      "pageId": "<previous page uuid>"
   }
```

### question-answered

The `question-answered` event is fired every 10 seconds if changes in answers are detected, ensuring nothing is lost in case the user abandons the interview. Below, you can find the `question-answered` schema:

```javascript
   type: "question-answered"
   detail: {
      "pageId": "<page uuid>",
      "pageTitle": "<page title when available>"
   }
```

### page-changed

The `page-changed` event is fired by the RocketDocument Embedded UX once it receives a successful response from the RLBE, meaning that all answers were actually saved. This is useful if the RocketDocument is going to show a success message. It is also fired at the last question of the interview since the RocketDocument will receive a response from the RLBE indicating that all answers were saved. Below, you can find the `page-changed` schema: 

```javascript
   type: "page-changed"
   detail: {
      "pageId": "<page uuid>",
      "pageTitle": "<page title when available>"
   }
```

### interview-completing

The `interview-completing` event indicates that the completion of the interview has started. At this point, the user is at the end of the interview, and the "Continue" button is disabled since there are no more questions to answer. Below, you can find the `interview-completing` schema: 

```javascript
   type: "interview-completing"
```

### interview-completed 

The `interview-completed` event is the last event fired by the RocketDocument Embedded UX. This means that the interview is completed and filled with all the info provided by the user. It will also tell the `ParentUI` that no further processes are running or yet remain to be completed. It is up to the `ParentUI` to take the user to the next screen since the RocketDocument only fires the event. Below, you can find the `interview-completed` schema: 

```javascript
   type: "interview-completed"
   detail: {
      "binderId": "<binder uuid>"
   }
```

### rocketdocumenterror

The `rocketdocumenterror` event is deprecated but retained for backward compatibility. The new event is `interview-error`. This event is fired when something bad happens while performing certain actions, especially when calling the RLBE. Below, you can find the `rocketdocumenterror` schema:

```javascript
   type: "interview-error"
   detail: {
      "code": "<code>",
      "message": "<string with the error msg>",
      "deprecated": true
   }
```

### interview-error

The `interview-error` event is fired when something bad happens while performing certain actions, especially when calling the RLBE. Below, you can find the `interview-error` schema:

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

### component-connected

The `component-connected` event is related to the lifecycle method `connectedCallback()`, which will be implemented by this component and called by Stencil when connected to the DOM. Below, you can find the `component-connected` schema:

```javascript
   type: "component-connected"
```

### component-disconnected

The `component-disconnected` event is related to the lifecycle method `disconnectedCallback()`, which will be implemented by this component and called by Stencil when disconnected from the DOM. Below, you can find the `component-disconnected` schema:

```javascript
   type: "component-disconnected"
```

> **Deprecated Events Notice:**
> 
> - The `interview-started` event has been deprecated in favor of the `interview-loaded` event. 
> - The `rocketdocumenterror` event has been deprecated in favor of `interview-error` to better describe the nature of errors.
