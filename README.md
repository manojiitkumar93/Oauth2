*How does OAuth 2 work? What does it mean to implement OAuth 2 authentication and authorization?*
Mainly, OAuth 2 refers to using **tokens** for authorization. Tokens are like access cards, once you obtain a token, you can access specific resources. But OAuth 2 offers multiple possibilities for obtaining a token, called **grants**

**Here are the most common OAuth 2 grants you can choose from**

#### Authorization Code Grant Type
The Authorization Code Grant Type is probably the most common of the OAuth 2.0 grant types that you’ll encounter. 

Technically, when the client redirects the user to the authorization server, the client calls the authorization endpoint with the following details in the request query..
- **response_type:** with the value *code* , which tells the authorization server that the client expects a code. The client needs the code to obtain an access token.
- **client_id:** with the value of the *client ID*, which identifies the application itself.
- **redirect_uri:** which tells the authorization server where to redirect the user after successful authentication. Sometimes the authorization server already knows a default redirect URI for each client. In such cases, the client doesn’t need to send the redirect URI
- **scope:** I is s one or more space-separated strings indicating which permissions the application is requesting
- **state:** The application generates a random string and includes it in the request. It should then check that the same value is returned after the user authorizes the app. This is used to prevent CSRF attacks.

After successful authentication, the authorization server calls back the client on the redirect URI and provides a code and the state value. The client checks that the state value is the same as the one it sent in the request to confirm that it was not someone
else attempting to call the redirect URI.

The resulting code is the client’s proof that the user authenticated. Now the client calls the authorization server with the
code to get the token. *So why didn’t the authorization server directly return the second token (access
token)?*

OAuth 2 defines a flow called the **implicit grant type** where the authorization server directly returns an access token. This implicit grant type is not recommended, and most authorization servers today don’t allow it. The simple fact that the authorization server would call the redirect URI directly with an access token without making sure that it was indeed the right client receiving that token makes the flow less secure. By sending an authorization code first, the client has to prove again who they are by using their credentials to obtain an access token. The client makes a final call to get an access token and sends
- The authorization code, which proves the user authorized them
- Their credentials, which proves they really are the same client and not someone else who intercepted the authorization codes

