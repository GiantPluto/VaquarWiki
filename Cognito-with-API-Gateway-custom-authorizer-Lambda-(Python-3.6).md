The advantage of using Lambda Function is that it can perform authorization processing other than verification of IdToken. For example, you can write processing according to your application, such as IP restrictions and allowing only specific user agents.

1) Create Cognito user pool:


The script when changing the password looks something like this. Please fill in the necessary items accordingly. After execution, I think that I can confirm that IdToken etc is output to the console.

       #!/bin/sh
 
      user_pool_id=""
      client_id=""
      user_name=""
      password=""
      new_password=""
 
     session="$(aws cognito-idp admin-initiate-auth --user-pool-id ${user_pool_id} --client-id ${client_id} --auth-flow 
     ADMIN_NO_SRP_AUTH --auth-parameters USERNAME=${user_name},PASSWORD=${password} | jq -r .Session)"
     aws cognito-idp admin-respond-to-auth-challenge --user-pool-id  ${user_pool_id} --client-id ${client_id} --challenge- 
     name NEW_PASSWORD_REQUIRED --challenge-responses USERNAME=${user_name},NEW_PASSWORD=${new_password} --session 
     ${session}





Reference site

https://aws.amazon.com/jp/premiumsupport/knowledge-center/decode-verify-cognito-json-token/
https://github.com/awslabs/aws-support-tools/tree/master/Cognito/decode-verify-jwt
https://github.com/awslabs/aws-apigateway-lambda-authorizer-blueprints
