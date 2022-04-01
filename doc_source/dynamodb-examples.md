--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Amazon DynamoDB Examples<a name="dynamodb-examples"></a>

Amazon DynamoDB is a fully managed NoSQL cloud database that supports both document and key\-value store models\. You create schemaless tables for data without the need to provision or maintain dedicated database servers\.

![\[Relationship between JavaScript environments, the SDK, and DynamoDB\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/code-samples-dynamodb.png)

The JavaScript API for DynamoDB is exposed through the `AWS.DynamoDB`, `AWS.DynamoDBStreams`, and `AWS.DynamoDB.DocumentClient` client classes\. For more information about using the DynamoDB client classes, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html), [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDBStreams.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDBStreams.html), and [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html) in the API reference\.

**Topics**
+ [Creating and Using Tables in DynamoDB](dynamodb-examples-using-tables.md)
+ [Reading and Writing A Single Item in DynamoDB](dynamodb-example-table-read-write.md)
+ [Reading and Writing Items in Batch in DynamoDB](dynamodb-example-table-read-write-batch.md)
+ [Querying and Scanning a DynamoDB Table](dynamodb-example-query-scan.md)
+ [Using the DynamoDB Document Client](dynamodb-example-document-client.md)