**GET:**

     var https = require('https');
           exports.handler = (event, context, callback) => {
           var params = {
                        host: "bittrex.com",
                        path: "/api/v1.1/public/getmarketsummaries"
        
                        };
   
      var req = https.request(params, function(res) {
        let data = '';
        console.log('STATUS: ' + res.statusCode);
        res.setEncoding('utf8');
        res.on('data', function(chunk) {
            data += chunk;
        });
        res.on('end', function() {
            console.log("DONE");
            console.log(JSON.parse(data));
        });
      });
       req.end();
     };

---------------------------------------------------------------------------

**POST:**



          var querystring = require('querystring');
          var http = require('http');

           exports.handler = function (event, context) {
           var post_data = querystring.stringify(
           event.body //TODO here we can get data fro cognito and then instead of obj need to set post_data into call
             );

           console.log('post_data='+post_data);

               //TODO need to get data fom cognito
              var obj = {
             "firstName": "zk",
             "invitationDate": "2019-03-16T05:19:47.307Z",
             "invitationExpiryDate": "2019-04-16T05:19:47.307Z",
             "lastName": "khan",
             "loginId": "Zkhan@gmail.com",
              "playerId": "90",
              "status": "1",
              "userId": "90"
           };

          console.log(JSON.stringify(obj));


      // An object of options to indicate where to post to
     var post_options = {
         host: 'dummyurl.ca-central-1.elasticbeanstalk.com',
         path: '/user/update',
         method: 'POST',
         headers: {
             'Content-Type': 'application/json',
             'Content-Length': Buffer.byteLength(JSON.stringify(obj))//TODO post_data
           }
     };

        console.log('post_options'+post_options);


       // Set up the request
       var post_req = http.request(post_options, function(res) {
        res.setEncoding('utf8');
         res.on('data', function (chunk) {
          console.log('Response: ' + chunk);
          context.succeed();
        });
         res.on('error', function (e) {
        console.log("Got error: " + e.message);
        context.done(null, 'FAILURE');
      });

      });
     console.log('post_req'+post_req);

       // post the data
        post_req.write(JSON.stringify(obj));//TODO post_data
        post_req.end();

      }