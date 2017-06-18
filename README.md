# Alternative CloudFormation templates for Serverless & GraphQL Course

## Why?
The [serverless framework](https://serverless.com/) does a great job of simplifying the steps required to configure API gateway, lambda, dynamodb etc., by generating the cloudformation for you. The challenge with this approach is when something doesn't work or the framework doesn't quite do what you need it to do.

The [Serverless Framework with GraphQL](https://acloud.guru/learn/serverless-with-graphql) is an excellent course that teaches you how to use the both the Serverless Framework and GraphQL to build serverless applications. As part of that course you setup a CloudFront distribution and you can map your own DNS name to this site, but there are a few complexities to doing this that can be painful to change and debug within the serverless framework.

After trying to resolve these issues I reverted to doing it by hand through the console and then writing the CloudFormation templates to do this. This solved these issues:
- Deploy lambda to a different region to us-east-1
- Avoid lengthy rollback when something went wrong with provisioning the CloudFront distribution or DNS

## How?
- In us-east 1 run the acm template. This will not complete until you have confirmed via the email prompt that you own the domain.
- In the region where you want to deploy the serverless application, run the CDN template and pass in the ARN for the certificate
- In that same region, run the DNS template and pass in the CDN domain name fron the output of the CDN template

## Assumptions
- The DNS template assumes you have already have your domain setup as a hosted zone with SOA and NS records.

## Possible improvements
- CDN template should export the DNS and CDN domain name and the DNS template should import this
- Add another template to also provision the Cognito part of the course
- Add another template to create the hosted zone for your domain
- Have a master template to run all (but the ACM since that might be in a different region) templates
