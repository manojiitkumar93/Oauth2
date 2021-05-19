*How does OAuth 2 work? What does it mean to implement OAuth 2 authentication and authorization?*
Mainly, OAuth 2 refers to using **tokens** for authorization. Tokens are like access cards, once you obtain a token, you can access specific resources. But OAuth 2 offers multiple possibilities for obtaining a token, called **grants**


#### Authorization Code Grant Type
The Authorization Code Grant Type is probably the most common of the OAuth 2.0 grant types that you’ll encounter. 

**----------------------------------------------------->   Step 1    <-----------------------------------------------------------**

Technically, when the client redirects the user to the authorization server, the client calls the authorization endpoint with the following details in the request query..
- **response_type:** with the value *code* , which tells the authorization server that the client expects a code. The client needs the code to obtain an access token.
- **client_id:** with the value of the *client ID*, which identifies the application itself.
- **redirect_uri:** which tells the authorization server where to redirect the user after successful authentication. Sometimes the authorization server already knows a default redirect URI for each client. In such cases, the client doesn’t need to send the redirect URI
- **scope:** I is s one or more space-separated strings indicating which permissions the application is requesting
- **state:** The application generates a random string and includes it in the request. It should then check that the same value is returned after the user authorizes the app. This is used to prevent CSRF attacks.

After successful authentication, the authorization server calls back the client on the redirect URI and provides a code and the state value. The client checks that the state value is the same as the one it sent in the request to confirm that it was not someone else attempting to call the redirect URI.

**----------------------------------------------------->   Step 2    <-----------------------------------------------------------**

The resulting code is the client’s proof that the user authenticated. Now the client calls the authorization server with the
code to get the token. *So why didn’t the authorization server directly return the second token (access
token)?*

OAuth 2 defines a flow called the **implicit grant type** where the authorization server directly returns an access token. This implicit grant type is not recommended, and most authorization servers today don’t allow it. The simple fact that the authorization server would call the redirect URI directly with an access token without making sure that it was indeed the right client receiving that token makes the flow less secure. By sending an authorization code first, the client has to prove again who they are by using their credentials to obtain an access token. The client makes a final call to get an access token and sends
- The authorization code, which proves the user authorized them
- Their credentials, which proves they really are the same client and not someone else who intercepted the authorization codes

In this step 2, technically, the client now makes a request to the authorization server. This request contains the following details:
**code :** which is the authorization code received in step 1. This proves that the user authenticated.
**client_id and client_secret :** the client’s credentials.
**redirect_uri :** which is the same one used in step 1 for validation.
**grant_type :** with the value authorization_code , which identifies the kind of flow used. A server might support multiple flows, so it’s essential always to specify which is the current executed authentication flow.

As a response, the server sends back an **access_token** . This token is a value that the client can use to call resources exposed by the resource server.

**----------------------------------------------------->   Step 3    <-----------------------------------------------------------**

After successfully obtaining the access token from the authorization server, the client can now call for the protected resource. The client uses an access token in the authorization request header when calling an endpoint of the resource server.

#### Password grant type 
This grant type is also known as the **resource owner credentials grant type**. Applications using this flow assume that the client collects the user credentials and uses these to authenticate and obtain an access token from the authorization server.

**----------------------------------------------------->   Step 1    <-----------------------------------------------------------**

The flow is much simpler with the **password grant type**. The client collects the user credentials and calls the authorization server to obtain an access token. When requesting the access token, the client also sends the following details in the request..
- **grant_type** with the value **password**
- **client_id and client_secret :** which are the credentials used by the client to authenticate itself.
- **scope :** which you can understand as the granted authorities.
- **username and password :**  which are the user credentials. These are sent in plain text as values of the request header.

The client receives back an access token in the response. The client can now use the access token to call the endpoints of the resource server.

**----------------------------------------------------->   Step 2    <-----------------------------------------------------------**

Once the client has an access token, it uses the token to call the endpoints on the resource server, which is exactly like the authorization code grant type. The client adds the access token to the requests in the authorization request header.

#### Client Credentials Grant Type
This grant type is used when no user is involved; that is, when implementing authentication between two applications.

**----------------------------------------------------->   Step 1    <-----------------------------------------------------------**

To obtain an access token, the client sends a request to the authorization server with the following details:
- **grant_type :** with the value **client_credentials**
- **client_id and client_secret :**  which represent the client credentials
- **scope :** which represents the granted authorities

In response, the client receives an access token. The client can now use the access token to call the endpoints of the resource server.

**----------------------------------------------------->   Step 2    <-----------------------------------------------------------**

Once the client has an access token, it uses that token to call the endpoints on the resource server, which is exactly like the authorization code grant type and the pass- word grant type. The client adds the access token to the requests in the authorization
request header.


