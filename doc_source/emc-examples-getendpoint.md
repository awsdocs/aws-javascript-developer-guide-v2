--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Getting Your Region\-Specific Endpoint for MediaConvert<a name="emc-examples-getendpoint"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve your region\-specific endpoint from MediaConvert\.

## The Scenario<a name="emc-example-getendpoint-scenario"></a>

In this example, you use a Node\.js module to call MediaConvert and retrieve your region\-specific endpoint\. You can retrieve your endpoint URL from the service default endpoint and so do not yet need your region\-specific endpoint\. The code uses the SDK for JavaScript to retrieve this endpoint by using this method of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#describeEndpoints-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#describeEndpoints-property)

**Important**  
The default Node\.js HTTP/HTTPS agent creates a new TCP connection for every new request\. To avoid the cost of establishing a new connection, the AWS SDK for JavaScript reuses TCP connections\. For more information, see [Reusing Connections with Keep\-Alive in Node\.js](node-reusing-connections.md)\.

## Prerequisite Tasks<a name="emc-example-getendpoint-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set Up IAM Permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

## Getting Your Endpoint URL<a name="emc-example-getendpoint-url"></a>

Create a Node\.js module with the file name `emc_getendpoint.js`\. Be sure to configure the SDK as previously shown\.

Create an object to pass the empty request parameters for the `describeEndpoints` method of the `AWS.MediaConvert` client class\. To call the `describeEndpoints` method, create a promise for invoking an MediaConvert service object, passing the parameters\. Handle the response in the promise callback\. 

```
// Load the SDK for JavaScript.
const aws = require('aws-sdk');

// Set the AWS Region.
aws.config.update({region: 'us-west-2'});

// Create the client.
const mediaConvert = new aws.MediaConvert({apiVersion: '2017-08-29'});

exports.handler = async (event, context) => {
  // Create empty request parameters
  const params = {
    MaxResults: 0,
  };

  try {
    const { Endpoints } = await mediaConvert.describeEndpoints(params).promise();
    console.log("Your MediaConvert endpoint is ", Endpoints);
  } catch (err) {
    console.log("MediaConvert Error", err);
  }
}
```

To run the example, type the following at the command line\.

```
node emc_getendpoint.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascript/example_code/mediaconvert/emc_getendpoint.js)\.