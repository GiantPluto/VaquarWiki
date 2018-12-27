Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0.

![Alt Text](https://docs.aws.amazon.com/apigateway/latest/developerguide/images/custom-auth-workflow.png)


Authorizers are the first line of defense to secure API , AWS cognito pool allowed you to use following two different type of authorizers.

1. Cognito Authorizers
2. Custom Lambda authorizer

![Alt Text](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2017/09/27/Screen-Shot-2017-09-13-at-10.13.18-AM-215x300.png)


AWS introduced a new type of authorizer in Amazon API Gateway, enhanced request authorizers. Previously, custom authorizers received only the bearer token included in the request and the ARN of the API Gateway method being called. Enhanced request authorizers receive all of the headers, query string, and path parameters as well as the request context. This enables you to make more sophisticated authorization decisions based on parameters such as the client IP address, user agent, or a query string parameter alongside the client bearer token.

So Authorizers can use token and request.


TOKEN type Lambda authorizers grant a caller permissions to invoke a given request using an authorization token passed in a header. The token could be, for example, an OAuth token.


Token-based lambda authorizers receive the below object as its event to process the authorization request:

     `{`
         `"type":"TOKEN",`
         `"authorizationToken":"<caller-supplied-token>",`
         `"methodArn":"arn:aws:execute-api:<regionId>:<accountId>:<apiId>/<stage>/<method>/<resourcePath>"`
       `}`



REQUEST type Lambda authorizers grant a caller permissions to invoke a given request using request parameters, including headers, query strings, stage variables, or context parameters.

For request-based lambda authorizers, lambda receives the event object as below:

     `{`
         `"type":"REQUEST",`
         `"methodArn":"arn:aws:execute-api:us-east-1:xxxx:xxxx/test/GET/request",`
         `"resource":"/request",`
        `"path":"/request",`
        `"httpMethod":"GET",`
        `"headers":{`
         `...`
         `},`
         `"queryStringParameters":{`
         `"QueryString1":"queryValue1"`
         `},`
         `"pathParameters":{`
         `},`
          `"stageVariables":{`
          `"StageVar1":"stageValue1"`
         `},`
        `"requestContext":{`
         `"path":"/request",`
         `"accountId":"xxxxx",`
         `"resourceId":"xxxx",`
         `"stage":"test",`
         `"requestId":"...",`
        `"identity":{`
         `"apiKey":"...",`
         `"sourceIp":"..."`
         `},`
         `"resourcePath":"/request",`
         `"httpMethod":"GET",`
         `"apiId":"xxxx"`
         `}`
      `}`



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

[Control a Lambda Function of a Lambda Authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html#api-gateway-lambda-authorizer-types)





## Sample Function python 2.7 

- https://github.com/vaquarkhan/aws-lambda-example
- https://medium.com/@awskarthik82/part-1-securing-aws-api-gateway-using-aws-cognito-oauth2-scopes-410e7fb4a4c0
-----------------------------------------------------------------------------------------------------

Above notes i have created using  aws doc,image and following links

-------------------------------------------------------------------------------------------------------
- https://markpollmann.com/lambda-authorizer/
- https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html
- https://blog.codecentric.de/en/2018/04/aws-lambda-authorizer/
- https://github.com/awslabs/aws-apigateway-lambda-authorizer-blueprints/blob/master/blueprints/java/src/io/TokenAuthorizerContext.java
- https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html
- https://aws.amazon.com/blogs/compute/using-enhanced-request-authorizers-in-amazon-api-gateway/
- https://dzone.com/articles/reuse-authorizers-across-aws-apis
- https://stackoverflow.com/questions/40656761/custom-authorizer-vs-cognito-authentication-for-amazon-api-gateway-web-appli
- https://auth0.com/docs/integrations/aws-api-gateway/custom-authorizers/part-3
- https://blog.codecentric.de/en/2018/04/aws-lambda-authorizer/
