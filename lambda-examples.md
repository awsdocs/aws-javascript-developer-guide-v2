# AWS Lambda Examples<a name="lambda-examples"></a>

AWS Lambda is a service that provides on\-demand compute capacity that runs in response to events without the need to provision or maintain a server\. With Lambda, you code computing tasks as Lambda functions, which can be called as needed across the Internet\. You can write Lambda functions using Node\.js\.

![\[Relationship between JavaScript environments, the SDK, and Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/code-samples-lambda.png)

For more information about Node\.js programming in Lambda, see [ Programming Model \(Node\.js\)](http://docs.aws.amazon.com/lambda/latest/dg/programming-model.html) in the *AWS Lambda Developer Guide*\.

## Using Lambda in Web Pages<a name="using-lambda-in-web-pages"></a>

The JavaScript API for Lambda is exposed through the `AWS.Lambda` client class\. For more information about using the Lambda client class, see [Class: AWS\.Lambda](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Lambda.html) in the API reference\. Create an instance of the `AWS.Lambda` class to give your browser script a service object that can invoke a Lambda function\. Create the JSON parameters you must pass to the `AWS.Lambda.invoke` method, which calls the Lambda function\.

Any data returned by the Lambda function is returned in the callback mechanism you use\. See [Calling Services Asychronously](calling-services-asynchronously.md) for details about different ways to set up a callback mechanism\.

**Topics**
+ [Using Lambda in Web Pages](#using-lambda-in-web-pages)
+ [Invoking a Lambda Function in a Browser Script](browser-invoke-lambda-function-example.md)
+ [Writing a Lambda Function in Node\.js](nodejs-write-lambda-function-example.md)