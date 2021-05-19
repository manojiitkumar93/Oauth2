*How does OAuth 2 work? What does it mean to implement OAuth 2 authentication and authorization?*
Mainly, OAuth 2 refers to using **tokens** for authorization. Tokens are like access cards, once you obtain a token, you can access specific resources. But OAuth 2 offers multiple possibilities for obtaining a token, called **grants**

**Here are the most common OAuth 2 grants you can choose from**

#### Authorization Code Grant Type
The Authorization Code Grant Type is probably the most common of the OAuth 2.0 grant types that you’ll encounter. At a high level, the flow has the following steps..

- The application opens a browser to send the user to the authorization server
- The user sees the authorization prompt and approves the app’s request
- The user is redirected back to the application with an authorization code in the query string
- The application exchanges the authorization code for an access token

Technically, when the client redirects the user to the authorization server, the client calls the authorization endpoint with the following details in the request query..
- **response_type:** with the value *code* , which tells the authorization server that the client expects a code. The client needs the code to obtain an access token.
- **client_id:** with the value of the *client ID*, which identifies the application itself.
- **redirect_uri:** which tells the authorization server where to redirect the user after successful authentication. Sometimes the authorization server already knows a default redirect URI for each client. In such cases, the client doesn’t need to send the redirect URI
- **scope:** I is s one or more space-separated strings indicating which permissions the application is requesting
- **state:** The application generates a random string and includes it in the request. It should then check that the same value is returned after the user authorizes the app. This is used to prevent CSRF attacks.
