# Configuring the SDK for JavaScript<a name="configuring-the-jssdk"></a>

Before you use the SDK for JavaScript to invoke web services using the API, you must configure the SDK\. At a minimum, you must configure these settings:

+ The *region* in which you will request services\.

+ The *credentials* that authorize your access to SDK resources\.

In addition to these settings, you may also have to configure permissions for your AWS resources\. For example, you can limit access to an Amazon S3 bucket or restrict an Amazon DynamoDB table for read\-only access\.

The topics in this section describe various ways to configure the SDK for JavaScript for Node\.js and JavaScript running in a web browser\.


+ [Using the Global Configuration Object](global-config-object.md)
+ [Setting the AWS Region](setting-region.md)
+ [Getting Your Credentials](getting-your-credentials.md)
+ [Setting Credentials](setting-credentials.md)
+ [Locking API Versions](locking-api-versions.md)
+ [Node\.js Considerations](node-js-considerations.md)
+ [Browser Script Considerations](browser-js-considerations.md)
+ [Bundling Applications with Webpack](webpack.md)