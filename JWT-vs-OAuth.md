
JWT is a simple authentication protocol, Oauth is an authentication framework.

-----------------------------------------------------------------------

JWT :
JWT stands for JSON Web Token as the name suggest it is only a token for transferring secured data among two parties, that is client and server.


![Alt Text](https://uploads.toptal.io/blog/image/125552/toptal-blog-image-1520247295599-b6253511e5f77cc92c6ccc019206e6e0.png)




- http://jwt.io/
- https://auth0.com/docs
- http://www.toptal.com/web/cookie-free-authentication-with-json-web-tokens-an-example-in-laravel-and-angularjs
- https://espeo.eu/blog/app-security-jwt-based-authentication/


OAUTH:
![Alt Text](https://i.stack.imgur.com/dboTu.png)

Oauth2, on the other hand, is a set of rules or a procedure commonly called a framework that help to authenticate and authorize two parties to transfer secured data.

Here is a more detailed explanation of the steps in the diagram:

1 The application requests authorization to access service resources from the user.

2 If the user authorized the request, the application receives an authorization grant.

3 The application requests an access token from the authorization server (API) by presenting authentication of its own identity, and the authorization grant.

4 If the application identity is authenticated and the authorization grant is valid, the authorization server (API) issues an access token to the application. Authorization is complete.

5 The application requests the resource from the resource server (API) and presents the access token for authentication.

6 If the access token is valid, the resource server (API) serves the resource to the application
Both these can be used together in transferring secure data.

**Where JWT come into play in 3rd 6th steps of oauth2**


- http://tutorials.jenkov.com/oauth2/overview.html


**When to use OAuth or JWT ?**

-https://www.quora.com/Should-I-use-OAuth2-or-JWT-for-my-API

--------------------------------------------------------


- https://stackoverflow.com/questions/39909419/jwt-vs-oauth-authentication
- https://softwareengineering.stackexchange.com/questions/298973/rest-api-security-stored-token-vs-jwt-vs-oauth

