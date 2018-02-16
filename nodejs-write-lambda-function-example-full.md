# Writing a Lambda Function Code<a name="nodejs-write-lambda-function-example-full"></a>

Here is the Node\.js code for the Lambda function example\.

```
// Configuring the AWS SDK
var AWS = require('aws-sdk');
AWS.config.update({region: 'REGION'});

exports.handler = (event, context, callback) => {

const URL_BASE = "S3_BUCKET_URL";

// Define the object that will hold the data values returned
var slotResults = {
	'isWinner' : false,
	'leftWheelImage' : {'file' : {S: ''}},
	'middleWheelImage' : {'file' : {S: ''}},
	'rightWheelImage' : {'file' : {S: ''}}
};

// define parameters JSON for retrieving slot pull data from the database
var thisPullParams = {
    Key : {'slotPosition' : {N: ''}},
    TableName: 'slotWheels',
    ProjectionExpression: 'imageFile'
};

// create DynamoDB service object
var request = new AWS.DynamoDB({region: 'REGION', apiVersion: '2012-08-10'});

// set a random number 0-9 for the slot position
thisPullParams.Key.slotPosition.N = Math.floor(Math.random()*10).toString();
// call DynamoDB to retrieve the image to use for the Left slot result
var myLeftPromise = request.getItem(thisPullParams).promise().then(function(data) {return URL_BASE + data.Item.imageFile.S});

// set a random number 0-9 for the slot position
thisPullParams.Key.slotPosition.N = Math.floor(Math.random()*10).toString();
// call DynamoDB to retrieve the image to use for the Left slot result
var myMiddlePromise = request.getItem(thisPullParams).promise().then(function(data) {return URL_BASE + data.Item.imageFile.S});

// set a random number 0-9 for the slot position
thisPullParams.Key.slotPosition.N = Math.floor(Math.random()*10).toString();
// call DynamoDB to retrieve the image to use for the Left slot result
var myRightPromise = request.getItem(thisPullParams).promise().then(function(data) {return URL_BASE + data.Item.imageFile.S});


Promise.all([myLeftPromise, myMiddlePromise, myRightPromise]).then(function(values) {
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

};
```