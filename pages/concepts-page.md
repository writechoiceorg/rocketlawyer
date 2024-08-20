
Understanding some foundational concepts about Rocket Lawyer's architecture is crucial for effectively integrating with its services. This page presents the essential concept definitions you need to know as you begin the integration process.
# Table of Contents
1. [**General Terms**](glossary#general-terms)   
2. [**RocketDocument™**](glossary#rocketdocument)   
3. [**RocketSign™**](glossary#rocketsign)   
4. [**Authentication**](glossary#authentication)  

<a name="general-terms"></a>
# General Terms
In this section, we provide some general terms:
- **API Endpoints**: Specific paths in Rocket Lawyer's API that allow developers to integrate legal services into their applications.  
- **Client Credentials**: Authentication information obtained through onboarding, including an API Key and Secret used to access Rocket Lawyer's APIs. The client credentials are available at the [Developer Portal](https://developer.rocketlawyer.com/accounts/login).
- **Embedded UX**: A user interface integrated into a partner’s web application, allowing seamless access to Rocket Lawyer’s services.
- **Event:** An action that occurs during the lifecycle of the legal process, such as sending an invitation to sign, a party viewing the document, or an owner modifying the document. These actions generate events that partners can subscribe to and retrieve using the events API.  
- **Universal Party ID (UPID)**: Used to specify the parties on a binder that have access to a document.

<a name="rocketdocument"></a>
# RocketDocument™
**RocketDocument** is Rocket Lawyer's document creation solution. Below, you find the definitions of related terms:
- **RocketDocument Embedded UX**: This embedded user interface allows users to interact with and fill out documents directly within your platform. The process is conducted as an interview, presenting a new question to the user for each editable field. This functionality allows you to obtain all user information and fill in all fields from different types of documents.
- **RocketDocument API Endpoints**: A set of APIs that programmatically manage the document creation and filling process. With the RocketDocument API, you can choose a document template, start an interview, navigate through all questions to fill out the document and complete the interview to access the final document.
- **Interview (interviewId):** A guided interview in a question-and-answer session that adjusts based on the user's answers, providing a tailor-made document creation experience. It can have two different storage types:  
  - **Persistent Interview:** A persistent interview sends each answer the user fills to RocketLawyer's servers. The data is stored there, and when the user finishes the interview, the data is used to generate the completed document.
  - **Ephemeral Interview**: For ephemeral interviews, the answers your customers provide in the interview process are not persisted on Rocket Lawyer's servers. Interview answers are still sent to Rocket Lawyer to determine questions to ask and to generate document previews and the completed document. 
- **Document:** This is the final artifact generated from a completed interview. It seamlessly merges the static content from the template with the dynamic content provided by the user's answers.  
- **Tagged Answer Model (TAM):** The TAM is a structure representing the data that a specific Document Template contains. In the TAM, you can input answers that you already have or retrieve data from interviews that have already been filled out.

<a name="rocketsign"></a>
# RocketSign™
**RocketSign** is Rocket Lawyer’s digital signing solution. Below, you find the definitions of related terms:
- **RocketSign Embedded UX**: An embedded user interface that allows users to interact with and sign documents directly within your platform. This feature enables document preparation, signing, and management to be seamlessly integrated into your application.  
- **RocketSign API Endpoints**: A set of APIs that programmatically manage the document signing process and binder-related tasks, including preparing documents, managing parties, and tracking the status of the legal process.  
- **Party:** An individual or entity involved in the legal document, either as a signer or participant.  
- **Binder (binderId):** Similar to a three-ring binder, it organizes and holds the document, details about each party, the status of the legal process, and all associated signatures. 

<a name="authentication"></a>
# Authentication
Explore definitions for terms related to **Authentication**:
- **Authentication API:** The Authentication API manages access to resources across Rocket Lawyer's APIs, ensuring secure and controlled interactions. For more information about the Authentication API, refer to the [Authentication API page](https://developer.rocketlawyer.com/docs/partner-auth-service-product-sandbox/1/overview).  
- **Access Token:** Your backend systems use an Access Token to enable deep integrations with Rocket Lawyer services, such as requesting lists of interviews or binders and obtaining access tokens for specific binders. This token is used for all back-end requests. For more information on how to work with Access Token, refer to the [Authentication API page](https://developer.rocketlawyer.com/docs/partner-auth-service-product-sandbox/1/overview).  
- **Service Token:** The Service Token is an intermediary for creating a Scoped Access Token. You will need an Access Token to create a Service Token and the Service Token to create a Scoped Access Token. For more information on how to work with Service Token, refer to the [Authentication API page](https://developer.rocketlawyer.com/docs/partner-auth-service-product-sandbox/1/overview).  
- **Scoped Access Token:** When building your own UI, your front end must create and use a scoped access token to call our APIs. This token is scoped either to the specific interview or the universal party ID of the user, meaning that it can only be used to access the information it’s scoped for. Separate front-end credentials are required to create scoped access tokens. 

