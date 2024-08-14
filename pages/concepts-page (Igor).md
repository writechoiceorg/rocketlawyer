Understanding some foundational concepts about Rocket Lawyer's architecture is crucial for effectively integrating with its services. This page presents the essential concept definitions you need to know as you begin the integration process.

## Table of Contents

1. [**General Terms**](#general-terms)   
2. [**RocketDocument™**](#rocketdocument)   
3. [**RocketSign™**](#rocketsign)   
4. [**Authentication**](#authentication)  
5. [**Identifiers**](#identifiers)

## General Terms

- **API Endpoints**: Specific paths in Rocket Lawyer's API that allow developers to integrate legal services into their applications.

- **Embedded UX**: A user interface integrated into a partner’s web application, allowing seamless access to Rocket Lawyer’s services.

- **iFrame**: An HTML element used to embed RocketSign Embedded UX into a webpage. It displays the RocketSign interface and allows users to interact with it directly within your platform.

## RocketDocument™

In this section, we provide definitions for terms related to **RocketDocument™**:

- **RocketDocument™:** This product enables customers to create and customize legal documents through a dynamic, interactive interview process. The product consists in both an Embedded UX and the API endpoints. RocketDocument™ is designed to smooth the document creation process by guiding users through an interactive interview to collect specific details for the document.

- **Interview:** A guided interview is a question-and-answer session that adjusts based on the user's answers, providing a tailor-made document creation experience.

- **Document Template:** This serves as the blueprint for a legal document, containing static legal content, the layout for dynamic content, and control logic that dictates the flow and customization of questions during the interview.

- **Document:** This is the final product generated from a completed interview. It seamlessly merges the static content from the template with the dynamic content provided by the user's answers.

## RocketSign™

**RocketSign** is Rocket Lawyer’s digital signing solution, comprising two main components:

1. **RocketSign Embedded UX**: An embedded user interface that allows users to interact with and sign documents directly within your platform. This feature enables document preparation, signing, and management seamlessly within your application.

2. **RocketSign API Endpoints**: A set of APIs that manage the document signing process and binder-related tasks programmatically, including preparing documents, managing parties, and tracking the status of the legal process.

Below we provide definitions for terms related to the **RocketSign Embedded UX** and the **RocketSign API Endpoints**:

### RocketSign Embedded UX

Below is the list of definitions for terms related to the  **RocketSign Embedded UX**:

- **RocketSign Embedded UX**: A feature that allows users to interact with and sign documents within your platform. An embedded user interface enables document preparation, signing, and management.

- **Party:** An individual or entity involved in the legal document, either as a signer or participant.

- **Binder:** Similar to a three-ring binder, it organizes and holds the document, details about each party, the status of the legal process, and all associated signatures.

- **Event:** Any activity that occurs during the lifecycle of the legal process, such as sending an invitation to sign, a party viewing the document, or an owner modifying the document.

### RocketSign API Endpoints

Below is the list of definitions for terms related to the  **RocketSign API Endpoints**:

- **RocketSign & Binders API**: This is an API for managing document signing processes and binder-related tasks, including preparing and signing documents.

- **Client Credentials**: Essential authentication details obtained through the onboarding process, including an API Key and Secret used for accessing Rocket Lawyer's APIs.

## Authentication

In this section, we provide definitions for terms related to **Authentication**:

- **Authentication API:** The Authentication API manages access to resources across Rocket Lawyer's APIs, ensuring secure and controlled interactions. For more information about the Authentication API, refer to the [Authentication API page](https://developer.rocketlawyer.com/docs/partner-auth-service-product-sandbox/1/overview).

- **Access Token:** Your backend systems use an Access Token to enable deep integrations with Rocket Lawyer services, such as requesting lists of interviews or binders and obtaining access tokens for specific binders. You use the `AccessTokenRequest` endpoint to get the Access Token. For more information on how to work with Access Token, refer to the [Authentication API page](https://developer.rocketlawyer.com/docs/partner-auth-service-product-sandbox/1/overview).

- **Service Token:** Your client applications use a Service Token to activate Rocket Lawyer UX components, enabling users to interact with features such as creating and signing legal documents. You use the `ServiceTokenRequest` endpoint to get the Service Token. For more information on how to work with Service Token, refer to the [Authentication API page](https://developer.rocketlawyer.com/docs/partner-auth-service-product-sandbox/1/overview).

- **Backend Access Token:** This token is intended for use by your backend systems and allows deep integration with Rocket Lawyer data. It is obtained through a POST request to the Authentication API and authorizes backend API calls.

- **UPID (Universal Party ID)**: UPID stands for Universally Unique IDentifier of the Party viewing the document. It is used to specify the party in requests related to service tokens. For more information about the UPID, refer to the [RocketDocument Embedded UX Quick Start guide](https://developer.rocketlawyer.com/rocketsign-embedded-ux).

## **Identifiers**

In this section, we provide definitions for terms related to **Identifiers**:

- **templateId:** This is a unique identifier for a document template used to initialize an interview. It defines the legal document's structure, static content, and customization logic.

- **partnerEndUserId:** This is a unique identifier used within your system to represent the end-user interacting with the Rocket Lawyer platform.

- **interviewId:** This is a unique identifier for a specific interview session, representing a legal document customization process based on the user's responses.

- **partyEmailAddress:** The end user's email address for document notifications.

- **binderId**: This is a unique identifier for a binder used to display and manage documents. It can be obtained by following the [RocketDocument Embedded UX Quick Start guide](https://developer.rocketlawyer.com/rocketdocument-embedded-ux).
