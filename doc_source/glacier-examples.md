--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Amazon S3 Glacier Examples<a name="glacier-examples"></a>

Amazon S3 Glacier is a secure cloud storage service for data archiving and long\-term backup\. The service is optimized for infrequently accessed data where a retrieval time of several hours is suitable\.

![\[Relationship between JavaScript environments, the SDK, and S3 Glacier\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/code-samples-glacier.png)

The JavaScript API for Amazon S3 Glacier is exposed through the `AWS.Glacier` client class\. For more information about using the S3 Glacier client class, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Glacier.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Glacier.html) in the API reference\.

**Topics**
+ [Creating a S3 Glacier Vault](glacier-example-creating-a-vault.md)
+ [Uploading an Archive to S3 Glacier](glacier-example-uploadrchive.md)
+ [Doing a Multipart Upload to S3 Glacier](glacier-example-multipart-upload.md)