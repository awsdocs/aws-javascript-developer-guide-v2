# What Is the AWS SDK for JavaScript?<a name="welcome"></a>

The AWS SDK for JavaScript provides a JavaScript API for AWS services you can use to build applications for [Node\.js](https://nodejs.org/en/) or the browser\. The JavaScript API lets developers build libraries or applications that make use of AWS services\.

![\[Relationship between JavaScript environments, the SDK, and Amazon Web Services\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/sdk-overview.png)

Not all services are immediately available in the SDK\. To find out which services are currently supported by the AWS SDK for JavaScript, see [ https://github\.com/aws/aws\-sdk\-js/blob/master/SERVICES\.md]( https://github.com/aws/aws-sdk-js/blob/master/SERVICES.md)\. For information about the SDK for JavaScript on GitHub, see [Additional Resources](resources.md)\.

## Using the SDK with Node\.js<a name="w3ab1b5b9"></a>

Node\.js a cross\-platform runtime for running server\-side JavaScript applications\. You can set up Node\.js on an Amazon EC2 instance to run on a server\. You can also use Node\.js to write on\-demand AWS Lambda functions\.

Using the SDK for JavaScript for Node\.js differs from using it for JavaScript in a web browser in the way you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, those differences will be called out\.

## Using the SDK with AWS Cloud9<a name="w3ab1b5c11"></a>

You can also develop Node\.js applications using the SDK for JavaScript in the AWS Cloud9 IDE\. For a sample of how to use AWS Cloud9 for Node\.js development, see [Node\.js Sample for AWS Cloud9](http://docs.aws.amazon.com/cloud9/latest/user-guide//sample-nodejs.html) in the *AWS Cloud9 User Guide*\. For more information on using AWS Cloud9 with the SDK for JavaScript, see [Using AWS Cloud9 with the AWS SDK for JavaScript](cloud9-javascript.md)\.

## Using the SDK with AWS Amplify<a name="w3ab1b5c13"></a>

For browser\-based web, mobile and hybrid apps, you can also use the [AWS Amplify Library on GitHub](https://github.com/aws/aws-amplify), which extends the SDK for JavaScript, providing a declarative interface\.

## Using the SDK with Web Browsers<a name="w3ab1b5c15"></a>

All major web browsers support execution of JavaScript\. JavaScript code running in a web browser is often called client\-side JavaScript\.

Using the SDK for JavaScript in a web browser differs from using it for Node\.js in the way you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, those differences will be called out\.

For browser-based web, mobile and hybrid apps, you can alternatively use the [AWS Amplify Library](https://aws.github.io/aws-amplify/?utm_source=aws-js-sdk&utm_campaign=browser) which extends the SDK for JavaScript and provides an easier and declarative interface.

For a list of browsers supported by the AWS SDK for JavaScript, see [Web Browsers Supported](browsers-supported.md)\.

### Common Use Cases<a name="w3ab1b5c15b8"></a>

Using the SDK for JavaScript in browser scripts makes it possible to realize a number of compelling use cases\. Here are several ideas for things you can build in a browser application using the SDK for JavaScript to access different web services\.
+ Building a custom console to AWS services in which you access and combine features across regions and services to best meet your organizational or project needs\.
+ Using Amazon Cognito Identity to enable authenticated user access to your browser applications and websites, including use of third\-party authentication from Facebook and others\.
+ Using Amazon Kinesis to process click streams or other marketing data in real time\.
+ Using Amazon DynamoDB for serverless data persistence such as individual user preferences for visitors of your website or application users\.
+ Using AWS Lambda to encapsulate proprietary logic that you can invoke from browser scripts without downloading and revealing your intellectual property to users\.
