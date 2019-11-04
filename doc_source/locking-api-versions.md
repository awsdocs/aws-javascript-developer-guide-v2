# Locking API Versions<a name="locking-api-versions"></a>

AWS services have API version numbers to keep track of API compatibility\. API versions in AWS services are identified by a `YYYY-mm-dd` formatted date string\. For example, the current API version for Amazon S3 is `2006-03-01`\.

We recommend locking the API version for a service if you rely on it in production code\. This can isolate your applications from service changes resulting from updates to the SDK\. If you don't specify an API version when creating service objects, the SDK uses the latest API version by default\. This could cause your application to reference an updated API with changes that negatively impact your application\.

To lock the API version that you use for a service, pass the `apiVersion` parameter when constructing the service object\. In the following example, a newly created `AWS.DynamoDB` service object is locked to the `2011-12-05` API version:

```
var dynamodb = new AWS.DynamoDB({apiVersion: '2011-12-05'});
```

You can globally configure a set of service API versions by specifying the `apiVersions` parameter in `AWS.Config`\. For example, to set specific versions of the DynamoDB and Amazon EC2 APIs along with the current Amazon Redshift API, set `apiVersions` as follows:

```
AWS.config.apiVersions = {
  dynamodb: '2011-12-05',
  ec2: '2013-02-01',
  redshift: 'latest'
};
```

## Getting API Versions<a name="getting-api-versions"></a>

To get the API version for a service, see the *Locking the API Version* section on the service's reference page, such as [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html) for Amazon S3\.