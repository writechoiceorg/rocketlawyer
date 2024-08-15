# Rocket Lawyer Authentication Tokens

Authentication tokens play a critical role in securing access to Rocket Lawyer’s API services, ensuring that only authorized users can interact with sensitive resources. This guide provides an overview of the Access Token, Service Token, and Scoped Access Token, detailing their use cases and instructions on how to implement them effectively during the document interview process.

- [Access Token](#access-token)
- [Service Token](#service-token)
- [Scoped Access Token](#scoped-access-token)
- [Using Authentication Tokens During the Interview Process](#using-authentication-tokens-during-the-interview-process)

## Access Token

The Access Token is fundamental to API authentication within Rocket Lawyer’s ecosystem, facilitating secure communication between your application and Rocket Lawyer’s API services. This secure, time-sensitive token is generated using the client ID and client secret from an app registered on the Rocket Lawyer Developer Portal. It serves as both an authentication and authorization tool, providing your application with the necessary access to Rocket Lawyer’s extensive suite of services.

You will rely on the Access Token whenever your application needs to perform tasks involving Rocket Lawyer’s API, such as starting new interviews, retrieving document templates, or managing documents across multiple users. To use the Access Token, generate it through the Authentication API using your app’s credentials. Once obtained, include the Access Token in the Authorization header of your API requests to ensure secure and authenticated interactions with Rocket Lawyer’s services.

## Service Token

The Service Token is a specialized token used to secure interactions related to specific users or sessions within Rocket Lawyer’s API ecosystem. This temporary token is generated for a particular user or session, using parameters such as the purpose, interview ID, and Unique Party Identifier (UPID). The primary function of a Service Token is to facilitate the creation of a Scoped Access Token, enabling more granular control over resource access.

Use a Service Token when you need to restrict access to specific documents or interviews within the Rocket Lawyer platform, ensuring that only authorized users can interact with these resources. To generate a Service Token, make a request to the authentication endpoint with the necessary parameters. Once the Service Token is obtained, it can be used to create a Scoped Access Token, which enforces limited access to the designated resources.

## Scoped Access Token

The Scoped Access Token is designed to provide secure, restricted access to specific resources, making it especially suitable for frontend interactions where maintaining security is paramount. This token grants controlled access to particular documents or interviews linked to a specific user, identified by their Unique Party Identifier (UPID). It is generated using a Service Token, ensuring that access is carefully managed and limited.

A Scoped Access Token should be used when frontend applications need to securely interact with specific documents or interviews, ensuring that only the authorized user can access these resources. To obtain a Scoped Access Token, first, generate a Service Token, and then use it to request the Scoped Access Token from the authentication API. This token should be included in the Authorization header of your frontend API requests to ensure secure, user-specific access to the required resources.

## Using Authentication Tokens During the Interview Process

During the interview process, different authentication tokens are used to manage and secure the interaction from start to finish, ensuring that user data and documents are protected throughout.

### Initializing the Interview

Begin by generating an Access Token to authenticate the initial API requests. This token is used to start a new interview and retrieve the necessary document templates, establishing a secure foundation for the session.

### Securing the Interview Session

Once the interview is initiated, create a Service Token to secure the session further. This token is then used to generate a Scoped Access Token, ensuring that access to the interview and associated documents is limited to the specific user.

### Completing the Interview

As the interview progresses, use the Scoped Access Token to securely submit user responses and, ultimately, retrieve the final document. This ensures that each step of the interview process is securely managed, with appropriate access controls in place.

