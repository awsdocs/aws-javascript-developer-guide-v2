# Invoking a Lambda Function in a Browser Script<a name="browser-invoke-lambda-function-example"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/browsericon.png)

**This browser script example shows you:**
+ How to create a Lambda service object used to invoke a Lambda function\.
+ How to access data values from JSON returned by the Lambda function\.

## The Scenario<a name="browser-invoke-lambda-function-example-scenario"></a>

In this example, a simulated slot machine browser\-based game invokes a Lambda function that generates the random results of each slot pull, returning those results as the file names of images used to display the result\. The images are stored in an Amazon S3 bucket that is configured to function as a static web host for the HTML, CSS, and other assets needed to present the application experience\.

![\[JavaScript running in a browser invoking an Lambda function.\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/invoking-lambda-from-browser.png)

## Prerequisite Tasks<a name="browser-invoke-lambda-function-example-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Download the \.zip archive that contains the assets needed for this app from [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/lambda/tutorial/slotassets.zip)\.
+ In the [Amazon S3 console](https://console.aws.amazon.com/s3/), create an Amazon S3 bucket configured to serve as a static web host\. Upload the HTML page, CSS file, and application graphics to the bucket\.
+ In the [Amazon Cognito console](https://console.aws.amazon.com/cognito/), create an Amazon Cognito identity pool with access enabled for unauthenticated identities\. You need to include the identity pool ID in the code to obtain credentials for the browser script\.
+ In the [IAM console](https://console.aws.amazon.com/iam/), create an IAM role for the Identity Pool to use\. Select Web Identity as the trusted entity, Cognito as the Identity Provider and input your identity pool ID from the previous step\. Next attach a policy that grants permission to invoke a Lambda function\. Finally, attach the policy to your Identity Pool as the unauthenticated role\.
+ Create the Lambda function called by the browser script that returns the result of each game spin\.

## Configuring the SDK<a name="browser-invoke-lambda-function-example-sdk"></a>

Here is the portion of the browser script that configures the SDK for JavaScript, using Amazon Cognito to obtain credentials\. This will return the unauthenticated role for the Identity Pool, the IAM role that we set up and attached as part of the prerequisites.

```
AWS.config.update({region: 'REGION'});
AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: 'IDENTITY_POOL_ID'});
```

## Creating the Lambda Service Object<a name="w4aac22c25c13c15"></a>

After configuring the SDK, this portion of the browser script creates a new Lambda service object, setting the region and API version\. After creating the service object, the code creates a JSON object for passing the parameters that are needed to invoke a Lambda function with the service object\. The code then creates a variable to hold the data returned by the Lambda function\.

```
var lambda = new AWS.Lambda({region: REGION, apiVersion: '2015-03-31'});
// create JSON object for parameters for invoking Lambda function
var pullParams = {
  FunctionName : 'slotPull',
  InvocationType : 'RequestResponse',
  LogType : 'None'
};
// create variable to hold data returned by the Lambda function
var pullResults;
```

## Invoking the Lambda Function<a name="browser-invoke-lambda-function-example-invoking-lambda-function"></a>

Later in the browser script, when the app is ready to request a random slot machine pull, the code calls the `invoke` method on the Lambda service object, passing the JSON object that holds the parameters that are needed to invoke the `slotPull` Lambda function\.

```
lambda.invoke(pullParams, function(error, data) {
  if (error) {
    prompt(error);
  } else {
    pullResults = JSON.parse(data.Payload);
  }
});
```

## Accessing the Returned Data<a name="browser-invoke-lambda-function-example-returned-data"></a>

In the preceding code example, the call to the Lambda function uses an anonymous function as the callback in which the response is received\. That response is automatically provided as the `data` parameter to the callback function\. That parameter passes the `data` property of the `AWS.Response` object received by the SDK\.

The `data` property contains the serialized data received from the Lambda function\. In this example, the Lambda function returns the results of each spin in the game as JSON\.

```
{ 
  isWinner: false,
  leftWheelImage : {S : 'cherry.png'},
  midWheelImage : {S : 'puppy.png'},
  rightWheelImage : {S : 'robot.png'}
}
```

To access the individual values contained within the `data` parameter, the following code turns the serialized data back into a JSON object by passing `data.Payload` to the `JSON.parse` function and then assigning the result to a variable\.

```
pullResults = JSON.parse(data.Payload);
```

After that, you can access individual data values in the JSON object by using `pullResults`\.

```
// check results to see if this spin is a winner
if (pullResults.isWinner) {
  prompt("Winner!");
}
```
