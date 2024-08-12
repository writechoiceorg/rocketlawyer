# RocketDoc UI and Web Component API Documentation

Welcome to the RocketDoc UI and Web Component API Documentation. This guide is designed to provide you with a comprehensive understanding of the RocketDoc UI embeddable component and its integration into your web applications. Whether you're a developer or a technical lead working with Rocket Lawyer's services, this documentation will walk you through the key events triggered during an interview process, explain how to manage these events effectively, and offer guidance on customizing the user interface to fit your specific needs.

This documentation is divided into two main sections:

1. **RocketDoc UI - Events and Interview Size**: In this section, you'll learn about the sequence of events that occur during an interview, how to listen for these events, and the error handling mechanisms in place. Additionally, it covers how to control the size of the RocketDoc UI component through CSS to ensure a smooth user experience within your application.

2. **RocketDoc V2 Web Component API Specification**: This section provides a detailed overview of the RocketDoc V2 Web Component, including setup instructions, component attributes, custom events, and CSS customization options. You'll also find examples of how to integrate and use the RocketDoc component within your web applications, along with recent updates and decisions that may impact your implementation.

By the end of this guide, you should have a clear understanding of how to integrate the RocketDoc UI into your application, customize its appearance and behavior, and handle the various events that it triggers. Whether you're setting up a new interview process or maintaining an existing integration, this documentation will serve as a valuable resource for ensuring a seamless and efficient user experience.

## **Table of Contents**

1. **RocketDoc UI - Events and Interview Size**
   - [Summary](#summary)
   - [Event Sequence](#event-sequence)
     - [1. Provide `serviceToken` & `interviewId`](#1-provide-servicetoken--interviewid)
     - [2. Fire `interview-loading` event](#2-fire-interview-loading-event)
     - [3. API Call to Get Interview & Template Information (`GET /interviews/{interviewId}`)](#3-api-call-to-get-interview--template-information-get-interviewsinterviewid)
     - [4. Fire `interview-started` event](#4-fire-interview-started-event)
     - [5. Render Interview](#5-render-interview)
     - [6. Fire `interview-loaded` event](#6-fire-interview-loaded-event)
     - [7. Display First Question](#7-display-first-question)
     - [8. Loop: User Answers Questions (for all but the last question)](#8-loop-user-answers-questions-for-all-but-the-last-question)
     - [9. Fire `interview-completing` event (after the last question is answered)](#9-fire-interview-completing-event-after-the-last-question-is-answered)
     - [10. API Call to Complete the Interview (`POST /interviews/{interviewId}/completions`)](#10-api-call-to-complete-the-interview-post-interviewsinterviewidcompletions)
     - [11. Fire `interview-completed` event](#11-fire-interview-completed-event)
   - [Error Handling with `rocketdocumenterror` / `interview-error`](#error-handling-with-rocketdocumenterror--interview-error)
   - [Listening to Events](#listening-to-events)
   - [Controlling the Interview Size](#controlling-the-interview-size)

2. **RocketDoc V2 Web Component API Specification**
   - [Overview](#overview)
   - [Package Overview](#package-overview)
     - [Hosts](#hosts)
     - [Target Audience](#target-audience)
     - [Authors and Reviewers](#authors-and-reviewers)
   - [Component Details](#component-details)
     - [`<rocket-document>` Component](#rocket-document-component)
   - [Attributes](#attributes)
   - [Example Usage](#example-usage)
   - [Custom Events](#custom-events)
     - [Key Events](#key-events)
   - [Event Listening Example](#event-listening-example)
   - [CSS Customization](#css-customization)
     - [CSS Variables](#css-variables)
     - [Example Customization](#example-customization)
     - [Additional Resources](#additional-resources)
   - [Updates and Decisions](#updates-and-decisions)
     - [Recent Changes](#recent-changes)

---

## RocketDoc UI - Events and Interview Size

### Summary
This document details the events fired by the RocketDoc UI embeddable component during an interview process, their significance, and how to handle them. It also explains how to control the size of the UI component through CSS to ensure a seamless user experience.

### Event Sequence
The event sequence outlined below applies to new interviews. When editing an existing interview, the order of events may vary. The RocketDoc UI component interacts with the RocketLawyer backend service (RLBE) throughout the process, ensuring efficient handling and rendering of the interview. The sequence is as follows:

#### 1. **Provide `serviceToken` & `interviewId`**
   - **Action:** The partner's front end initiates the process by providing the `serviceToken` and `interviewId` to the RocketDocument Embeddable UI component.
   - **Purpose:** These credentials are essential for fetching the interview data from the RocketDocument API.

#### 2. **Fire `interview-loading` event**
   - **Description:** This is the initial event fired by the RocketDoc UI component, occurring just before the API call is made to retrieve the interview data.
   - **Use Case:** You can listen to this event to manage loading states, such as displaying a custom loading indicator or hiding elements until the interview is ready.

#### 3. **API Call to Get Interview & Template Information (`GET /interviews/{interviewId}`)**
   - **Action:** The RocketDoc UI component sends a GET request to fetch the interview and template information using the provided `interviewId`.
   - **Purpose:** To gather all necessary data required for rendering the interview.

#### 4. **Fire `interview-started` event**
   - **Description:** This event is fired after the interview data is successfully retrieved, and the interview starts rendering in the background.
   - **Event Details:** The event contains the `interviewId`.
   - **Use Case:** This event can be used to trigger any pre-interview preparations or UI transitions needed in your application.

#### 5. **Render Interview**
   - **Action:** The RocketDoc UI component begins rendering the interview interface based on the retrieved data.
   - **Purpose:** To prepare the UI for displaying the interview questions.

#### 6. **Fire `interview-loaded` event**
   - **Description:** This event indicates that the interview has been fully loaded and is ready to display the first question.
   - **Use Case:** This is useful for showing the first question or for transitioning from a loading state to the active interview interface.

#### 7. **Display First Question**
   - **Action:** The first question of the interview is displayed to the user, initiating the interactive interview process.

#### 8. **Loop: User Answers Questions (for all but the last question)**
   - **Action:** Each time the user answers a question and clicks "Continue," the following occurs:
     - **Event Triggered:** The `question-answered` event is fired, which includes the `pageId`.
   - **Use Case:** Track progress through the interview, update UI elements, or trigger custom behaviors based on the current question.

#### 9. **Fire `interview-completing` event (after the last question is answered)**
   - **Description:** This event is fired when the user has answered the final question, signaling the start of the interview completion process.
   - **Use Case:** Prepare for finalizing the interview, such as saving data or transitioning to a summary screen.

#### 10. **API Call to Complete the Interview (`POST /interviews/{interviewId}/completions`)**
   - **Action:** The RocketDoc UI component sends a POST request to complete the interview.
   - **Purpose:** Finalize the interview process and ensure all user-provided information is saved.

#### 11. **Fire `interview-completed` event**
   - **Description:** The final event, indicating that the interview is fully completed with all necessary data submitted.
   - **Use Case:** This event allows you to transition the user to the next stage, such as a completion message or navigation to another part of the application.

### Error Handling with `rocketdocumenterror` / `interview-error`
The RocketDoc UI component handles errors by firing the `rocketdocumenterror` or the updated `interview-error` event. These events provide details about the error encountered. Below are the possible errors:

- **Load Interview Error (`LOAD_INTERVIEW_ERROR`)**
  - **Message:** "An error occurred loading the interview."
  
- **Save Error (`SAVE_ERROR`)**
  - **Message:** "An error occurred while saving interview answers. You may be offline. We will continue to attempt to save your answers."
  
- **Completion Connection Error (`COMPLETION_CONNECTION_ERROR`)**
  - **Message:** "An error occurred, and your document could not be completed."
  
- **Completion Error (`COMPLETION_ERROR`)**
  - **Message:** "An error occurred, and your document could not be completed."

### Listening to Events
To listen for these events, you can add JavaScript code to your page as follows:

```javascript
<script>
  document
    .querySelector("rocket-document")
    .addEventListener("interview-loading", (e) => {
      console.log("interview-loading event received", e.detail);
    });
</script>
```

The `rocket-document` tag defines the UI component, and listeners can be added for various events to execute custom code when those events are fired. The `e` variable in the listener callback provides details such as the `interviewId`, which can be useful for tracking and processing.

### Controlling the Interview Size
When integrating the RocketDoc UI component, you may want to define the size of the interview screen. This can be achieved by applying CSS properties such as height or padding. Here’s an example:

```css
<style type="text/css">
  rocket-document {
    padding-top: 20px;
    height: 600px;
  }
</style>
```

In this example, the height of the component is set, and top padding is added, making the interview content scrollable while maintaining the position of the bottom bar. This gives you the flexibility to adjust the component size as per your requirements.

---

## RocketDoc V2 Web Component API Specification

### Overview

The RocketDoc V2 Web Component API is designed to provide a seamless integration experience for Rocket Lawyer partners, enabling them to embed the RocketDoc interview process into their applications with minimal effort. This document outlines the API specification, including setup instructions, component attributes, events, and customization options.

### Package Overview

The main goal of the RocketDoc V2 Web Component is to offer a single JavaScript file that partners can include in their websites to run an interview process. The RocketDoc UI provides the following benefits:

- A user interface that presents every page with all questions and fields needed to generate the final document.
- Custom events fired by the component to allow the parent UI to react accordingly.
- Built-in logic to navigate from one page to the next.

#### Hosts

- **Sandbox:** [https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js](https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js)
- **Production:** [https://rocket-document.rocketlawyer.com/rocket-document.js](https://rocket-document.rocketlawyer.com/rocket-document.js)

#### Target Audience
This API is intended for use by Rocket Lawyer partners who wish to integrate the RocketDoc interview process into their web applications.

#### Authors and Reviewers
- **Author:** Ulises Estecche
- **Reviewers:** Chris Hodapp, Erik Altman, Ashok Subramanian, Brion Moss, Abhijeet Vijayakar, Sumit Malhotra, Amit Bagree, Mark Walker

### Component Details

#### <rocket-document> Component

The `<rocket-document>` component is the primary interface for embedding the RocketDoc UI within a partner's web application. This component handles all aspects of the interview process, including rendering questions and handling user input.

### Attributes

- **`serviceToken`**: A required access token that delegates permission to perform actions over a specific interview. This token includes brand-specific configurations for the UI.
  - **Scopes:** 
    - `api.rocketlawyer.com/binder-party-access` (for partners using RocketSign)
    - `api.rocketlawyer.com/rocketdoc` (for partners using RocketDocument)
  
- **`accessToken`**: Can be used instead of `serviceToken`. It is a scoped access token specific to the interview.
  
- **`interviewId`**: A required UUID that identifies the interview. This is used by the RocketDoc UI to display all associated questions and fields.
  
- **`pageId`**: An optional attribute that allows the caller to specify which page of the interview should be opened.
  - **Options:**
    - `first`: Opens the first page of the interview (default if `pageId` is not provided).
    - `last`: Opens the last updated page of the interview.
    - `$pageId`: Opens a specific page by its UUID.

### Example Usage

```html
<script src="https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js"></script>

<rocket-document
  serviceToken="your-service-token"
  interviewId="your-interview-id"
  pageId="first">
</rocket-document>
```

### Custom Events

The RocketDoc V2 UI fires several custom events during the interview process. These events enable the parent UI to respond dynamically to user interactions.

### Key Events

- **`interview-loading`**: Fired just before the RocketDoc UI calls the RLBE (Rocket Lawyer Backend) to fetch the interview.
  
- **`interview-started`**: Deprecated in favor of `interview-loaded` but retained for backward compatibility. Indicates that the interview data has been received and rendering has begun.
  
- **`interview-loaded`**: Indicates that the RocketDoc UI has received a valid interview from the API and is rendering it in the background.

- **`loading-next-page`**: Fired when the user clicks "Continue," before the RocketDoc UI calls the RLBE to load the next page.

- **`loading-previous-page`**: Fired when the user clicks "Back," before the RocketDoc UI calls the RLBE to load the previous page.

- **`question-answered`**: Fired periodically (every 10 seconds) to ensure that answers are saved, even if the user abandons the interview.

- **`page-changed`**: Fired after a successful response from the RLBE, confirming that answers were saved. This event also fires after the last question is answered.

- **`interview-completing`**: Indicates that the user has answered all questions, and the interview is in the process of being completed.

- **`interview-completed`**: The final event, indicating that the interview is complete, and no further actions are required.

- **`interview-error`**: Fired when an error occurs during the interview process. The `code` attribute in the event details provides specific error information (e.g., `LOAD_INTERVIEW_ERROR`, `SAVE_ERROR`).

### Event Listening Example

To listen for events fired by the RocketDoc component, add a JavaScript event listener to your page:

```javascript
<script>
  document
    .querySelector("rocket-document")
    .addEventListener("interview-loading", (e) => {
      console.log("interview-loading event received", e.detail);
    });
</script>
```

### CSS Customization

#### CSS Variables
Partners can customize the RocketDoc UI using CSS variables. The customization options include overriding default styles with partner-specific branding.

#### Example Customization

```css
<style type="text/css">
  rocket-document {
    padding-top: 20px;
    height: 600px;
  }
</style>
```

This CSS code sets the height of the RocketDoc UI component and adds padding to the top. The component will remain scrollable if the content exceeds the defined height.

#### Additional Resources

- **RocketDoc Embedded UI Stylesheet Spec**: Detailed documentation on all available CSS variables for further customization.

### Updates and Decisions

#### Recent Changes

- The `pageNumber` parameter inside events has been renamed to `pageId` to maintain consistency across the RocketDoc V2 platform.
- The `rocketdocumenterror` event has been deprecated in favor of `interview-error` to better describe the nature of errors.




