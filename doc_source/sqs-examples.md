--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Amazon SQS Examples<a name="sqs-examples"></a>

Amazon Simple Queue Service \(Amazon SQS\) is a fast, reliable, scalable, fully managed message queuing service\. Amazon SQS lets you decouple the components of a cloud application\. Amazon SQS includes standard queues with high throughput and at\-least\-once processing, and FIFO queues that provide FIFO \(first\-in, first\-out\) delivery and exactly\-once processing\.

![\[Relationship between JavaScript environments, the SDK, and Amazon SQS\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/code-samples-sqs.png)

The JavaScript API for Amazon SQS is exposed through the `AWS.SQS` client class\. For more information about using the Amazon SQS client class, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html) in the API reference\.

**Topics**
+ [Using Queues in Amazon SQS](sqs-examples-using-queues.md)
+ [Sending and Receiving Messages in Amazon SQS](sqs-examples-send-receive-messages.md)
+ [Managing Visibility Timeout in Amazon SQS](sqs-examples-managing-visibility-timeout.md)
+ [Enabling Long Polling in Amazon SQS](sqs-examples-enable-long-polling.md)
+ [Using Dead Letter Queues in Amazon SQS](sqs-examples-dead-letter-queues.md)