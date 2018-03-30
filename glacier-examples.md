# Amazon Glacier Examples<a name="glacier-examples"></a>

Amazon Glacier is a secure cloud storage service for data archiving and long\-term backup\. The service is optimized for infrequently accessed data where a retrieval time of several hours is suitable\.

![\[Relationship between JavaScript environments, the SDK, and Amazon Glacier\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/code-samples-glacier.png)

The JavaScript API for Amazon Glacier is exposed through the `AWS.Glacier` client class\. For more information about using the Amazon Glacier client class, see [Class: AWS\.Glacier](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Glacier.html) in the API reference\.

**Topics**
+ [Creating an Amazon Glacier Vault](glacier-example-creating-a-vault.md)
+ [Uploading an Archive to Amazon Glacier](glacier-example-uploadrchive.md)
+ [Doing a Multipart Upload to Amazon Glacier](glacier-example-multipart-upload.md)