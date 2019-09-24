# Using the Global Configuration Object<a name="global-config-object"></a>

There are two ways to configure the SDK:
+ Set the global configuration using `AWS.Config`\.
+ Pass extra configuration information to a service object\.

Setting global configuration with `AWS.Config` is often easier to get started, but service\-level configuration can provide more control over individual services\. The global configuration specified by `AWS.Config` provides default settings for service objects that you create subsequently, simplifying their configuration\. However, you can update the configuration of individual service objects when your needs vary from the global configuration\.

## Setting Global Configuration<a name="setting-global-configuration"></a>

After you load the `aws-sdk` package in your code you can use the `AWS` global variable to access the SDK's classes and interact with individual services\. The SDK includes a global configuration object, `AWS.Config`, that you can use to specify the SDK configuration settings required by your application\.

Configure the SDK by setting `AWS.Config` properties according to your application needs\. The following table summarizes `AWS.Config` properties commonly used to set the configuration of the SDK\.


****  

| Configuration Options | Description | 
| --- | --- | 
| credentials | Required\. Specifies the credentials used to determine access to services and resources\. | 
| region | Required\. Specifies the Region in which requests for services are made\. | 
| maxRetries | Optional\. Specifies the maximum number of times a given request is retried\. | 
| logger | Optional\. Specifies a logger object to which debugging information is written\. | 
| update | Optional\. Updates the current configuration with new values\. | 

For more information about the configuration object, see [ Class: AWS\.Config](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Config.html) in the API Reference\.

### Global Configuration Examples<a name="global-configuration-examples"></a>

You must set the Region and the credentials in `AWS.Config`\. You can set these properties as part of the `AWS.Config` constructor, as shown in the following browser script example:

```
var myCredentials = new AWS.CognitoIdentityCredentials({IdentityPoolId:'IDENTITY_POOL_ID'});
var myConfig = new AWS.Config({
  credentials: myCredentials, region: 'us-west-2'
});
```

You can also set these properties after creating `AWS.Config` using the `update` method, as shown in the following example that updates the Region:

```
myConfig = new AWS.Config();
myConfig.update({region: 'us-east-1'});
```

You can get your default credentials by calling the static `getCredentials` method of `AWS.config`:

```
var AWS = require("aws-sdk");

AWS.config.getCredentials(function(err) {
  if (err) console.log(err.stack);
  // credentials not loaded
  else {
    console.log("Access key:", AWS.config.credentials.accessKeyId);
    console.log("Secret access key:", AWS.config.credentials.secretAccessKey);
  }
});
```

Similarly, if you have set your region correctly in your `config` file, you get that value by setting the `AWS_SDK_LOAD_CONFIG` environment variable is set to a truthy value and calling the static `region` property of `AWS.config`:

```
var AWS = require("aws-sdk");

console.log("Region: ", AWS.config.region);
```

## Setting Configuration Per Service<a name="service-specific-configuration"></a>

Each service that you use in the SDK for JavaScript is accessed through a service object that is part of the API for that service\. For example, to access the Amazon S3 service you create the Amazon S3 service object\. You can specify configuration settings that are specific to a service as part of the constructor for that service object\. When you set configuration values on a service object, the constructor takes all of the configuration values used by `AWS.Config`, including credentials\.

For example, if you need to access Amazon EC2 objects in multiple Regions, create an EC2 service object for each Region and then set the Region configuration of each service object accordingly\.

```
var ec2_regionA = new AWS.EC2({region: 'ap-southeast-2', maxRetries: 15, apiVersion: '2014-10-01'});
var ec2_regionB = new AWS.EC2({region: 'us-east-1', maxRetries: 15, apiVersion: '2014-10-01'});
```

You can also set configuration values specific to a service when configuring the SDK with `AWS.Config`\. The global configuration object supports many service\-specific configuration options\. For more information about service\-specific configuration, see [Class: AWS\.Config](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Config.html) in the AWS SDK for JavaScript API Reference\.

## Immutable Configuration Data<a name="immutable-config"></a>

Global configuration changes apply to requests for all newly created service objects\. Newly created service objects are configured with the current global configuration data first and then any local configuration options\. Updates you make to the global `AWS.config` object don't apply to previously created service objects\.

Existing service objects must be manually updated with new configuration data or you must create and use a new service object that has the new configuration data\. The following example creates a new Amazon S3 service object with new configuration data:

```
s3 = new AWS.S3(s3.config);
```