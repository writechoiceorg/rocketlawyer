# Rocket Lawyer Authentication Tokens

Authentication tokens play a critical role in ensuring secure access to Rocket Lawyer’s API services. This page provides a comprehensive overview of the three main types of tokens—Access Token, Service Token, and Scoped Access Token—along with guidance on how to use these tokens effectively during the document interview process.

Explore the different authentication tokens and their usage within Rocket Lawyer's API ecosystem through the sections below:

- [Access Token](#access-token)
- [Service Token](#service-token)
- [Scoped Access Token](#scoped-access-token)
- [Using Authentication Tokens During the Interview Process](#using-authentication-tokens-during-the-interview-process)

## Access Token

An Access Token is a secure, server-to-server token created using a client key and secret. It is essential for authenticating API requests and granting broad access to Rocket Lawyer’s APIs. This token is primarily used for server-side interactions, enabling applications to perform actions such as managing interviews, retrieving documents, and handling multiple user sessions.

Access Tokens are used whenever an application needs to interact with Rocket Lawyer’s API services at a broad level. This includes starting new interviews, accessing all documents created by the app, or performing administrative tasks that involve multiple users.

To use an Access Token, generate it by calling the Authentication API with your app’s client key and secret. Once obtained, include the Access Token in the Authorization header of your API requests to ensure secure communication with Rocket Lawyer’s services.

## Service Token

A Service Token is a temporary token created for specific purposes, such as securing interactions related to individual users or sessions. This token is generated with parameters like the purpose, interview ID, and Unique Party Identifier (UPID). It is primarily used to generate Scoped Access Tokens, which provide more granular control over resource access.

Service Tokens are utilized when there’s a need to restrict access to specific documents or interviews. They ensure that only authorized users can interact with these resources by creating Scoped Access Tokens tied to individual users or specific sessions.

Generate a Service Token by sending a request to the authentication endpoint with the necessary parameters. Once generated, use the Service Token to create a Scoped Access Token, which enforces restricted access to designated resources.

## Scoped Access Token

A Scoped Access Token is a secure token designed for frontend interactions where security is paramount. It grants restricted access to specific resources, such as documents or interviews linked to a particular user (identified by a UPID). This token is created using a Service Token, ensuring that access is carefully managed and limited to the intended user.

Scoped Access Tokens are used in scenarios where frontend applications need to interact with specific documents or interviews securely. They ensure that only the authorized user can access these resources, making them ideal for frontend implementations that require strict access control.

To obtain a Scoped Access Token, first, generate a Service Token. Then, use the Service Token to request the Scoped Access Token from the authentication API. This token should be included in the Authorization header of your frontend API requests to maintain secure, user-specific access to the necessary resources.

## Using Authentication Tokens During the Interview Process

Authentication tokens are integral to managing the interview process securely and efficiently. Here’s how to use them:

### Initializing the Interview

Begin by generating an Access Token to authenticate the initial API requests. This token is used to start a new interview and retrieve the necessary document templates, establishing a secure foundation for the session.

### Securing the Interview Session

Once the interview is initiated, create a Service Token to secure the session further. This token is then used to generate a Scoped Access Token, ensuring that access to the interview and associated documents is limited to the specific user.

### Completing the Interview

As the interview progresses, use the Scoped Access Token to securely submit user responses and, ultimately, retrieve the final document. This ensures that each step of the interview process is securely managed, with appropriate access controls in place.


