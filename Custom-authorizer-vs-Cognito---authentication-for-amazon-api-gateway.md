Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0.

Authorizers are the first line of defense to secure API , AWS cognito pool allowed you to use following two different type of authorizers.

1. Cognito Authorizers
2. Custom Lambda authorizer

-----------------------------------------------------------------------------------------

## AWS Cognito:

Pros

AWS SDK handles everything for you and you cannot make much mistake in your authentication process.
Fine grained access control for AWS resources via IAM.
An extra lambda function in front of every API is not required for authentication.

Cons

Need to use AWS SDK specifically on client side. Programmers have to add this into their toolchain and make use if it during development. Adds extra complexity.
Fine grained access control for resources is not really required since the only access that is required is for API gateway.

## Custom Lambda authorizer

Pros

You can have your authentication mechanism the way you want it. Ultimate control over authentication and authorization.
You can have the UI call the APIs with a standard token (JWT) and the flow for developers remains same. No extra consideration of AWS SDK.

Cons

Authentication requires a lot of thinking and effort to build.
Chances of missing some crucial aspects are always there.
Its like reinventing the wheel. Why do it when Amazon has already done it for you.


Note : The authorizer is an intercepting mechanism provided so that you can add custom logic into lambda function and call in authorize calls. The custom logic may use rules based authorization.


-------------------------------------------------------------------------------------------------------
- https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html
- https://blog.codecentric.de/en/2018/04/aws-lambda-authorizer/
- https://github.com/awslabs/aws-apigateway-lambda-authorizer-blueprints/blob/master/blueprints/java/src/io/TokenAuthorizerContext.java
- https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html
- https://stackoverflow.com/questions/40656761/custom-authorizer-vs-cognito-authentication-for-amazon-api-gateway-web-appli
