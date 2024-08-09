## Concepts and Definitions

To effectively integrate with Rocket Lawyer's services, it's crucial to understand some foundational concepts about its architecture. This section will outline the essential elements you'll need to know as you begin the integration process.

### RocketDocument™

In this section, we provide definitions for terms related to RocketDocument™:

- **RocketDocument™:** This tool enables customers to create and customize legal documents through a dynamic, interactive interview process. It is designed to make the document creation process smooth by guiding users through an interactive interview to collect specific details for the document.

- **Interview:** A guided interview is a question-and-answer session that adjusts in real-time based on the user's answers, providing a tailor-made document creation experience.

- **Document Template:** This serves as the blueprint for a legal document, containing static legal content, the layout for dynamic content, and control logic that dictates the flow and customization of questions during the interview.

- **Document:** This is the final product generated from a completed interview. It seamlessly merges the static content from the template with the dynamic content provided by the user’s answers.

### RocketSign and Binders

In this section, we provide definitions for terms related to RocketSign and Binders:

- **RocketSign and Binders:** This service enables customers to sign documents and invite others to do the same. A binder is a digital container that stores information about the document and tracks all related activities.

- **Party:** An individual or entity involved in the legal document, either as a signer or participant.

- **Binder:** Similar to a three-ring binder, it organizes and holds the document, details about each party, the status of the legal process, and all associated signatures.

- **Event:** Any activity that occurs during the lifecycle of the legal process, such as sending an invitation to sign, a party viewing the document, or an owner modifying the document.

### Authentication API

In this section, we provide definitions for terms related to Authentication API:

- **Authentication API:** The Authentication API manages access to resources across Rocket Lawyer’s APIs, ensuring secure and controlled interactions.

- **Access Token:** Your backend systems use an Access Token to enable deep integrations with Rocket Lawyer services, such as requesting lists of interviews or binders and obtaining access tokens for specific binders.

- **Service Token:** Your client applications use a Service Token to activate Rocket Lawyer UX components, enabling users to interact with features such as creating and signing legal documents.

