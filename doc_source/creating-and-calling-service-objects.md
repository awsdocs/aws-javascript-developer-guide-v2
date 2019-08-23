# Creating and Calling Service Objects<a name="creating-and-calling-service-objects"></a>

The JavaScript API supports most available AWS services\. Each service class in the JavaScript API provides access to every API call in its service\. For more information on service classes, operations, and parameters in the JavaScript API, see the [API reference](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/)\.

When using the SDK in Node\.js, you add the SDK package to your application using `require`, which provides support for all current services\.

```
var AWS = require('aws-sdk');
```

When using the SDK with browser JavaScript, you load the SDK package to your browser scripts using the AWS\-hosted SDK package\. To load the SDK package, add the following `<script>` element:

```
<script src="https://sdk.amazonaws.com/js/aws-sdk-SDK_VERSION_NUMBER.min.js"></script>
```

To find the current SDK\_VERSION\_NUMBER, see the API Reference for the SDK for JavaScript at [https://docs\.aws\.amazon\.com/AWSJavaScriptSDK/latest/index\.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/)\.

The default hosted SDK package provides support for a subset of the available AWS services\. For a list of the default services in the hosted SDK package for the browser, see [Supported Services](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/#Supported_Services) in the API Reference\. You can use the SDK with other services if CORS security checking is disabled\. In this case, you can build a custom version of the SDK to include the additional services you require\. For more information on building a custom version of the SDK, see [Building the SDK for Browsers](building-sdk-for-browsers.md)\.

## Requiring Individual Services<a name="requiring-individual-services"></a>

Requiring the SDK for JavaScript as shown previously includes the entire SDK into your code\. Alternately, you can choose to require only the individual services used by your code\. Consider the following code used to create an Amazon S3 service object\.

```
// Import the AWS SDK
var AWS = require('aws-sdk');

// Set credentials and Region
// This can also be done directly on the service client
AWS.config.update({region: 'us-west-1', credentials: {YOUR_CREDENTIALS}});

var s3 = new AWS.S3({apiVersion: '2006-03-01'});
```

In the previous example, the `require` function specifies the entire SDK\. The amount of code to transport over the network as well as the memory overhead of your code would be substantially smaller if only the portion of the SDK you require for the Amazon S3 service was included\. To require an individual service, call the `require` function as shown, including the service constructor in all lower case\.

```
require('aws-sdk/clients/SERVICE');
```

Here is what the code to create the previous Amazon S3 service object looks like when it includes only the Amazon S3 portion of the SDK\.

```
// Import the Amazon S3 service client
var S3 = require('aws-sdk/clients/s3');
 
// Set credentials and Region
var s3 = new S3({
    apiVersion: '2006-03-01',
    region: 'us-west-1', 
    credentials: {YOUR_CREDENTIALS}
  });
```

You can still access the global AWS namespace without every service attached to it\.

```
require('aws-sdk/global');
```

This is a useful technique when applying the same configuration across multiple individual services, for example to provide the same credentials to all services\. Requiring individual services should reduce loading time and memory consumption in Node\.js\. When done along with a bundling tool such as Browserify or webpack, requiring individual services results in the SDK being a fraction of the full size\. This helps with memory or disk\-space constrained environments such as an IoT device or in a Lambda function\.

## Creating Service Objects<a name="creating-service-objects"></a>

To access service features through the JavaScript API, you first create a *service object* through which you access a set of features provided by the underlying client class\. Generally there is one client class provided for each service; however, some services divide access to their features among multiple client classes\.

To use a feature, you must create an instance of the class that provides access to that feature\. The following example shows creating a service object for DynamoDB from the `AWS.DynamoDB` client class\.

```
var dynamodb = new AWS.DynamoDB({apiVersion: '2012-08-10'});
```

By default, a service object is configured with the global settings also used to configure the SDK\. However, you can configure a service object with runtime configuration data that is specific to that service object\. Service\-specific configuration data is applied after applying the global configuration settings\.

In the following example, an Amazon EC2 service object is created with configuration for a specific Region but otherwise uses the global configuration\.

```
var ec2 = new AWS.EC2({region: 'us-west-2', apiVersion: '2014-10-01'});
```

In addition to supporting service\-specific configuration applied to an individual service object, you can also apply service\-specific configuration to all newly created service objects of a given class\. For example, to configure all service objects created from the Amazon EC2 class to use the US West \(Oregon\) \(`us-west-2`\) Region, add the following to the `AWS.config` global configuration object\.

```
AWS.config.ec2 = {region: 'us-west-2', apiVersion: '2016-04-01'};
```

## Locking the API Version of a Service Object<a name="locking-api-version-of-service-objects"></a>

You can lock a service object to a specific API version of a service by specifying the `apiVersion` option when creating the object\. In the following example, a DynamoDB service object is created that is locked to a specific API version\.

```
var dynamodb = new AWS.DynamoDB({apiVersion: '2011-12-05'});
```

For more information about locking the API version of a service object, see [Locking API Versions](locking-api-versions.md)\.

## Specifying Service Object Parameters<a name="specifying-service-object-parameters"></a>

When calling a method of a service object, pass parameters in JSON as required by the API\. For example, in Amazon S3, to get an object for a specified bucket and key, pass the following parameters to the `getObject` method\. For more information about passing JSON parameters, see [Working with JSON](working-with-json.md)\.

```
s3.getObject({Bucket: 'bucketName', Key: 'keyName'});
```

For more information about Amazon S3 parameters, see [Class: AWS\.S3](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html) in the API reference\.

In addition, you can bind values to individual parameters when creating a service object using the `params` parameter\. The value of the `params` parameter of service objects is a map that specifies one or more of the parameter values defined by the service object\. The following example shows the `Bucket` parameter of an Amazon S3 service object being bound to a bucket named `myBucket`\.

```
var s3bucket = new AWS.S3({params: {Bucket: 'myBucket'}, apiVersion: '2006-03-01' });
```

By binding the service object to a bucket, the `s3bucket` service object treats the `myBucket` parameter value as a default value that no longer needs to be specified for subsequent operations\. Any bound parameter values are ignored when using the object for operations where the parameter value isn't applicable\. You can override this bound parameter when making calls on the service object by specifying a new value\. 

```
var s3bucket = new AWS.S3({ params: {Bucket: 'myBucket'}, apiVersion: '2006-03-01' });
s3bucket.getObject({Key: 'keyName'});
// ...
s3bucket.getObject({Bucket: 'myOtherBucket', Key: 'keyOtherName'});
```

Details about available parameters for each method are found in the API reference\.