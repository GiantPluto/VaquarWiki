- https://vmokshagroup.com/blog/setting-up-a-secure-email-engine-using-amazon-ses/

## Amazon SES and lambda instance in different regions

SES not available in all region However, there is no reason you cannot connect cross-region to an SES service from your existing region. You do not need to change your app in your existing region to do this. While SES and networking best practices suggest you would want to choose an endpoint closest to your application, to reduce network latency, there is no SES requirement for your app instance to be in the same region as your SES service. Assuming you are using SMTP/S to send email, the email server can be anywhere on the internet.

You can configure your app in Asia-Pacific to send email via the SMTP/S endpoint provided by SES in any region. Again, network latency may be an issue, but depending on your mail volume, I would not expect network latency to be prohibitive. In fact I believe this setup is quite common for users in regions where SES is not supported.

I would suggest you setup the SES service in any of the available regions (say EU-Ireland), and run some basic load testing and see how the latency affects your application, if at all.

For more info, see Connecting to the Amazon SES SMTP Endpoint

---------------------------------------------------------
#### Use GMail with your own domain for free thanks to Amazon SES & Lambda

- http://www.daniloaz.com/en/use-gmail-with-your-own-domain-for-free-thanks-to-amazon-ses-lambda/