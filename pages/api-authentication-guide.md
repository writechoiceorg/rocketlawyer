In Rocket Lawyer’s API ecosystem, authentication tokens are crucial for ensuring secure access to services and resources. This guide provides detailed descriptions of the Access Token and Service Token, including their use cases, and explains how to use these tokens effectively during the interview process.

## Table of Contents

- [Access Token](#access-token)
   - [What is an Access Token?](#what-is-an-access-token)
   - [When to Use an Access Token](#when-to-use-an-access-token)
   - [How to Use the Access Token](#how-to-use-the-access-token)

- [Service Token](#service-token)
   - [What is a Service Token?](#what-is-a-service-token)
   - [When to Use a Service Token](#when-to-use-a-service-token)
   - [How to Use the Service Token](#how-to-use-the-service-token)

- [Using Authentication Tokens During the Interview Process](#using-authentication-tokens-during-the-interview-process)
   - [Initializing the Interview](#initializing-the-interview)
   - [Securing the Interview Session](#securing-the-interview-session)
   - [Completing the Interview](#completing-the-interview)

## Access Token

### What is an Access Token?

The Access Token is a secure string used to authenticate and authorize API requests. It is generated using the credentials (client ID and client secret) of an app registered on the Rocket Lawyer Developer Portal. This token is essential for server-to-server communication and provides broad access to various API services, such as creating interviews, retrieving document templates, and managing multiple users' documents.

### When to Use an Access Token

The Access Token is used whenever you need to perform actions across the Rocket Lawyer APIs, such as:

- **Starting a new interview:** When initializing a new interview session for a document.
- **Retrieving templates:** When obtaining a list of document templates to display to users.
- **Managing documents:** When accessing or modifying documents created by multiple users within the same application.

### How to Use the Access Token

1. **Obtain the Access Token:** Generate the token by calling the Authentication API with your app’s client ID and secret.
2. **Include in API Requests:** Use the Access Token in the Authorization header of your API requests to authenticate the calls.

## Service Token

### What is a Service Token?

The Service Token is a temporary token associated with a specific user or session. It is used to create a Scoped Access Token, which limits access to particular documents or interviews. This token is particularly useful when you need to restrict API access to certain resources tied to an individual user (identified by a Unique Person ID, or UPID).

### When to Use a Service Token

Service Tokens are used in scenarios where you need to:

- **Restrict access to specific documents:** Ensure that only authorized users can access or modify specific documents or interviews.
- **Create Scoped Access Tokens:** When preparing for frontend interactions, where security and limited access are critical.

### How to Use the Service Token

1. **Generate a Service Token:** Request a Service Token by specifying the purpose, associated interview ID, UPID, and expiration time.
2. **Use in Scoped Access Token Creation:** Utilize the Service Token to generate a Scoped Access Token for secure frontend use.

## Using Authentication Tokens During the Interview Process

### Initializing the Interview

1. **Generate an Access Token:** Start by obtaining a General Access Token to authenticate your API requests.
2. **Create the Interview:** Use the Access Token to initialize an interview by selecting the appropriate document template.

### Securing the Interview Session

1. **Generate a Service Token:** Once the interview is initiated, create a Service Token to ensure secure access to the interview.
2. **Create a Scoped Access Token:** Use the Service Token to generate a Scoped Access Token, which will be used in frontend applications to restrict access to the interview to the intended user.

### Completing the Interview

1. **Submit Answers:** As users go through the interview, use the Scoped Access Token to submit their answers securely.
2. **Retrieve the Document:** After completing the interview, use the Scoped Access Token to retrieve the final document.

By following these steps, you ensure that each part of the interview process is securely handled, with appropriate access controls in place for each user.