       'use strict';

        const AWS = require('aws-sdk');
        const cognitoIdp = new AWS.CognitoIdentityServiceProvider({apiVersion: '2016-04-18'});

        exports.handler = function(event, context) {
         console.log(JSON.stringify(event));

         // check if email is already in use
         if (event.request.userAttributes.hasOwnProperty('email')) {
         const email = event.request.userAttributes.email;
    
         const params = {
         UserPoolId: event.userPoolId,
         Filter: 'email = "' + email + '"',
        };
    
        cognitoIdp.listUsers(params).promise()
       .then (results => {
         console.log(JSON.stringify(results));
       / / if the usernames are the same, dont raise and error here so that
        // cognito will raise the duplicate username error
         if (results.Users.length > 0 && results.Users[0].Username !== event.userName) {
         console.log('Duplicate email address in signup. ' + email);
         context.done(Error('A user with the same email address exists'));
        }
        context.done(null, event);
       })
      .catch (error => {
      console.error(error);
      context.done(error);      
       });
     }
   };




        /*exports.handler = async (event) => {
           // TODO implement
             const response = {
            statusCode: 200,
            body: JSON.stringify('Hello from Lambda!'),
         };
        return response;
    };*/
