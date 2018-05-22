# Writing a Lambda Function in Node\.js<a name="nodejs-write-lambda-function-example"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to prepare an object representation of data that the function returns to the browser\.
+ How to create a service object used to access a DynamoDB table that stores the data values retrieved and returned by the Lambda function\.
+ How to return the requested data to the calling application as a JSON payload\.

## The Scenario<a name="w3ab1c22c25c15b9"></a>

In this example, a Lambda function supports a browser\-hosted slot machine game\. For the purposes of this example, we treat the JavaScript code running in the browser as a closed box\. When a player pulls the slot machine lever, the browser code invokes the Lambda function to generate the random results of each spin\. While this task is pretty simple, it illustrates one of the benefits of a Lambda function, which is that you can keep proprietary code hidden and separate from JavaScript code running in the browser\.

![\[Node.js running in a Lambda function providing database access to a browser application\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/nodejs-lambda-function.png)

The Lambda function, which uses Node\.js 4\.3, requests a randomly generated result for a browser\-based slot machine game\. Each time the function is invoked, a random number from 0 through 9, inclusive, is generated and used as the key to look up an image file from a DynamoDB table\. The three random results are compared to each other and if all three match, the spin is flagged as a winner\. The results of the spin are saved into a JSON object and returned to the browser script\.

The asynchronous requests to DynamoDB use separate calls to the `getItem` method of the DynamoDB service object instead of making a single call to the `getBatchItem` method\. The reason these are separate calls is because each of the three random results needed for each spin of the game can produce the same result\. In fact, a winner is defined by all three results being identical\. However the `getBatchItem` method returns an error if you request the same key from the table more than once in the same call\.

Because it's necessary to make three separate asynchronous requests to DynamoDB, the Lambda function can only return its results after receiving the response from all three method calls\.

## Prerequisite Tasks<a name="w3ab1c22c25c15c11"></a>

To set up and run this example, you must first complete these tasks:
+ Create an HTML page with browser script accessed in an Amazon S3 bucket acting as a static web host\. The browser script invokes the Lambda function\.
+ Create a DynamoDB table with a single row containing ten columns, each containing the file name of a different graphic stored in the Amazon S3 bucket used by the browser portion of the game\.
+ Create an IAM execution role for the Lambda function\.\.

Use the following role policy when creating the IAM role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem"
            ],
            "Resource": "arn:aws:dynamodb:*:*:*"
        }
    ]
}
```

## Configuring the SDK<a name="w3ab1c22c25c15c13"></a>

Here is the portion of the Lambda function that configures the SDK\. The credentials are not provided in the code because they are supplied to a Lambda function through the required IAM execution role\.

```
var AWS = require('aws-sdk');
AWS.config.update({region: 'us-west-2'});
```

## Preparing JSON Objects<a name="w3ab1c22c25c15c15"></a>

This Lambda function creates two different JSON objects\. The first of these JSON objects packages the data sent as the response of the Lambda function after it has successfully completed\. The second JSON object holds the parameters needed to call the `getItem` method of the DynamoDB service object\. The name\-value pairs of this JSON are determined by the parameters of `getItem`\.

```
// define JSON used to format Lambda function response
var slotResults = {
  'isWinner' : false,
  'leftWheelImage' : {'file' : {S: ''}},
  'middleWheelImage' : {'file' : {S: ''}},
  'rightWheelImage' : {'file' : {S: ''}}
};

// define JSON for making getItem calls to the slotWheels DynamoDB table	
var thisPullParams = {
    Key : {'slotPosition' : {N: ''}},
    TableName: 'slotWheels',
    ProjectionExpression: 'imageFile'
};
```

## Calling DynamoDB<a name="w3ab1c22c25c15c17"></a>

Each of the three calls to the `getItem` method of the DynamoDB service object is made and each response is managed using its own `Promise` object\. All three of these `Promise` objects are identical except for the name used to keep track of which result is used for which result in the game\.

The code generates a random number in the range 0 to 9, inclusive, and then assigns the number to the `Key` value in the JSON that is used to call DynamoDB\. It then calls the `getItem` method on the DynamoDB service object, passing in the JSON with the completed set of parameters\.

The Lambda function reads the `imageFile` column value returned by DynamoDB and concatenates that file name to a constant, forming the URL for an Amazon S3 bucket where the image files and the browser portion of the app are hosted\. The callback then resolves the `Promise` object, passing the URL\.

```
// create DynamoDB service object
var request = new AWS.DynamoDB({region: 'us-west-2', apiVersion: '2012-08-10'});

// set a random number 0-9 for the slot position
thisPullParams.Key.slotPosition.N = Math.floor(Math.random()*10).toString();
// call DynamoDB to retrieve the image to use for the Left slot result
var myLeftPromise = request.getItem(thisPullParams).promise().then(function(data) {return URL_BASE + data.Item.imageFile.S});

// set a random number 0-9 for the slot position
thisPullParams.Key.slotPosition.N = Math.floor(Math.random()*10).toString();
// call DynamoDB to retrieve the image to use for the Middle slot result
var myMiddlePromise = request.getItem(thisPullParams).promise().then(function(data) {return URL_BASE + data.Item.imageFile.S});

// set a random number 0-9 for the slot position
thisPullParams.Key.slotPosition.N = Math.floor(Math.random()*10).toString();
// call DynamoDB to retrieve the image to use for the Right slot result
var myRightPromise = request.getItem(thisPullParams).promise().then(function(data) {return URL_BASE + data.Item.imageFile.S});
```

## Gathering and Sending the Response<a name="w3ab1c22c25c15c19"></a>

The Lambda function can't send its response until all three of the asynchronous calls to DynamoDB have completed and returned their values\. To track the resolution of all three `Promise` objects used to call DynamoDB, another `Promise` object is used\. The callback of this `Promise` object is invoked by `Promise.all`, which takes an array of `Promise` objects as its parameters\. So this `Promise` resolves only if all of the `Promise` objects in the array resolve\. The callback receives an array holding the return values of resolved `Promise` objects as its parameter\.

The URL values returned by the `Promise` objects in the array are then assigned into the JSON created to send back the response of the Lambda function\. These three values are compared to determine if they are all identical\. If they are all identical, the fourth value in the returned JSON is set to `true`\.

After all four values in the returned JSON have been set, the JSON is passed to `callback`, which returns the results to the browser script and terminates the Lambda function\.

```
Promise.all([myLeftPromise, myMiddlePromise, myRightPromise]).then(function(values) {
  // assign resolved promise values to returned JSON
  slotResults.leftWheelImage.file.S = values[0];
  slotResults.middleWheelImage.file.S = values[1];
  slotResults.rightWheelImage.file.S = values[2];
  // if all three values are identical, the spin is a winner
  if ((values[0] === values[1]) && (values[0] === values[2])) {
    slotResults.isWinner = true;            
  }
  // return the JSON result to the caller of the Lambda function
  callback(null, slotResults);
});
```