# Prepare and Create the Lambda Function<a name="using-lambda-function-prep"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Tutorial: Creating and Using Lambda Functions](using-lambda-functions.md)\.

In this task, you will focus on creating the Lambda function used by the application\.

![\[JavaScript running in a browser invoking a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/create-lambda-function.png)![\[JavaScript running in a browser invoking a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/)![\[JavaScript running in a browser invoking a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/)

The Lambda function is invoked by the browser script every time the player of the game clicks and releases the handle on the side of the machine\. There are 16 possible results that can appear in each slot position, chosen at random\. 

The Lambda function generates three random results, one for each slot wheel in the game\. For each result, the Lambda function calls the Amazon DynamoDB table to retrieve the file name of the result graphic\. Once all three results are determined and their matching graphic URLs are retrieved, the result information is returned to the browser script for showing the result\.

The Node\.js code needed for the Lambda function is provided in the `slotassets.zip` archive file that you downloaded from the [code example archive on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/lambda/tutorial/slotassets.zip)\. You must edit this code to use it in your Lambda function\. Completing this portion of the application requires you to do these things:
+ Edit the Node\.js code used by the Lambda function\.
+ Compress the Node\.js code into a \.zip archive file that you then upload to the Amazon S3 bucket used by the application\.
+ Edit the Node\.js setup script for creating the Lambda function\.
+ Run the setup script, which creates the Lambda function from the \.zip archive file in the Amazon S3 bucket\.

**To make the necessary edits in the Node\.js code of the Lambda function**

1. Open the `slotassets.zip` archive that you downloaded from the [code sample archive on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/lambda/tutorial/slotassets.zip)\.

1. Copy `slotpull.js` to the folder that contains your credentials JSON file\.

1. Open `slotpull.js` in a text editor\.

1. Find this line of code in the browser script\.

   ```
   TableName: = "TABLE_NAME";
   ```

1. Replace *TABLE\_NAME* with the name you used to create the DynamoDB table\.

1. Save `slotpull.js`\.

**To prepare the Node\.js code for creating the Lambda function**

1. Compress `slotpull.js` into a \.zip archive file for creating the Lambda function\.

1. Upload `slotpull.js.zip` to the Amazon S3 bucket you created for this app\.

## Lambda Function Code<a name="using-lambda-function-code"></a>

For details about the example code in the Lambda function, see [Writing a Lambda Function in Node\.js](nodejs-write-lambda-function-example.md)\. The code creates a JSON object to package the result for the browser script in the application\. Next, it generates three random integer values that are used to look up items in the DynamoDB table, and obtains file names of images in the Amazon S3 bucket\. The result JSON is populated and passed back to the Lambda function caller\.

## Creating the Lambda Function<a name="using-lambda-function-creation"></a>

You can provide the Node\.js code for the Lambda function is in a file compressed into a \.zip archive file that you upload to an Amazon S3 bucket\. The `slotassets.zip` archive file contains a Node\.js setup script named `lambda-function-setup.js` that you modify and then run to create the Lambda function\.

**To edit the Node\.js setup script for creating the Lambda function**

1. Open the `slotassets.zip` archive file that you downloaded from the [code example archive on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/lambda/tutorial/slotassets.zip)\.

1. Copy `lambda-function-setup.js` to the folder that contains your credentials JSON file\.

1. Open `lambda-function-setup.js` in a text editor\. 

1. Find this line in the script 

   ```
      S3Bucket: 'BUCKET_NAME',
   ```

   Replace *BUCKET\_NAME* with the name of the Amazon S3 bucket that contains the \.zip archive file of the Lambda function code\.

1. Find this line in the script 

   ```
      S3Key: 'ZIP_FILE_NAME',
   ```

   Replace *ZIP\_FILE\_NAME* with the name of the \.zip archive file of the Lambda function code in the Amazon S3 bucket\.

1. Find this line in the script\. 

   ```
      Role: 'ROLE_ARN',
   ```

   Replace *ROLE\_ARN* with the ARN of the execution role you just created\.

1. Save your changes to `lambda-function-setup.js` and close the file\.

**To run the setup script and create the Lambda function from the \.zip archive file in the Amazon S3 bucket**
+ At the command line, type the following\.

  ```
  node lambda-function-setup.js
  ```

## Creation Script Code<a name="using-lambda-function-create-script"></a>

The following is the script that creates the Lambda function\. The code assumes you uploaded the \.zip archive file of the Lambda function to the Amazon S3 bucket you created for the application\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set region from JSON file
AWS.config.loadFromPath('./config.json');

// Create the IAM service object
var lambda = new AWS.Lambda({apiVersion: '2015-03-31'});


var params = {
  Code: { /* required */
    S3Bucket: ''BUCKET_NAME',
    S3Key: 'ZIP_FILE_NAME'
  },
  FunctionName: 'slotTurn', /* required */
  Handler: 'slotSpin.Slothandler', /* required */
  Role: 'ROLE_ARN', /* required */
  Runtime: 'nodejs8.10', /* required */
  Description: 'Slot machine game results generator'
};
lambda.createFunction(params, function(err, data) {
  if (err) console.log(err); // an error occurred
  else     console.log("success");           // successful response
});
```

This script uses example code from this AWS SDK for JavaScript topic:
+ [Writing a Lambda Function in Node\.js](nodejs-write-lambda-function-example.md)

## Next Step<a name="w4aac26b8c25c29"></a>

Return to the full [Tutorial Steps](using-lambda-functions.md#using-lambda-procedures)\.