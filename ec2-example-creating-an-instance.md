# Creating an Amazon EC2 Instance<a name="ec2-example-creating-an-instance"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**

+ How to create an Amazon EC2 instance from a public Amazon Machine Image \(AMI\)\.

+ How to create and assign tags to the new Amazon EC2 instance\.

## The Scenario<a name="ec2-example-creating-an-instance-scenario"></a>

In this example, you use a Node\.js module to create an Amazon EC2 instance and assign tags to it\. The code uses the SDK for JavaScript to create and tag an instance by using these methods of the Amazon EC2 client class:

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#runInstances-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#runInstances-property)

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createTags-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createTags-property)

## Prerequisite Tasks<a name="ec2-example-creating-an-instance-prerequisites"></a>

To set up and run this example, first complete these tasks:

+ Install Node\.js\. For more information, see the [Node\.js website](http://nodejs.org)\.

+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="ec2-example-creating-an-instance-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the region for your code\. 

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'us-west-2'});
```

## Creating and Tagging an Instance<a name="ec2-example-creating-an-instance-and-tags"></a>

Create a Node\.js module with the file name `ec2_createinstances.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Call the `runInstances,` method, and then call the `createTags` method of the Amazon EC2 service object\.

The code adds a `Name` tag to a new instance, which the Amazon EC2 console recognizes and displays in the **Name** field of the instance list\. You can add up to 50 tags to an instance, all of which can be added in a single call to the `createTags` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
   ImageId: 'ami-10fd7020', // amzn-ami-2011.09.1.x86_64-ebs
   InstanceType: 't1.micro',
   MinCount: 1,
   MaxCount: 1
};

// Create the instance
ec2.runInstances(params, function(err, data) {
   if (err) {
      console.log("Could not create instance", err);
      return;
   }
   var instanceId = data.Instances[0].InstanceId;
   console.log("Created instance", instanceId);
   // Add tags to the instance
   params = {Resources: [instanceId], Tags: [
      {
         Key: 'Name',
         Value: 'SDK Sample'
      }
   ]};
   ec2.createTags(params, function(err) {
      console.log("Tagging instance", err ? "failure" : "success");
   });
});
```

To run the example, type the following at the command line\.

```
node ec2_createinstances.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_createinstances.js)\.