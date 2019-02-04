What is the purpose of the "iat" field? For example, why would one want to determine the age of a JWT? Were there specific purposes in mind when the spec was created?

"iat" can help the service issued the JWT to some make decisions on their own instead of depending on the issuer for a fixed expiration time (suggested in the comment by Joe above). A JWT issuer could set both an expiration "exp" time (of say a fixed date several weeks away) as well as an issued at "iat" time - the service receiving the token could decide that the expiration time is much too long, and discard/expire it after a day (which it can compute with "iat").

- https://stackoverflow.com/questions/39926104/what-format-is-the-exp-expiration-time-claim-in-a-jwt/39926886
- https://tools.ietf.org/html/rfc7519#section-4.1.6