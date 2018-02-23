# Managing IAM Users<a name="iam-examples-managing-users"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**

+ How to retrieve a list of IAM users\.

+ How to create and delete users\.

+ How to update a user name\.

## The Scenario<a name="iam-examples-managing-users-scenario"></a>

In this example, a series of Node\.js modules are used to create and manage users in IAM\. The Node\.js modules use the SDK for JavaScript to create, delete, and update users using these methods of the `AWS.IAM` client class:

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createUser-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createUser-property)

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listUsers-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listUsers-property)

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateUser-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateUser-property)

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getUser-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getUser-property)

+ [http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteUser-property](http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteUser-property)

For more information about IAM users, see [IAM Users](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) in the *IAM User Guide*\.

## Prerequisite Tasks<a name="iam-examples-managing-users-prerequisites"></a>

To set up and run this example, you must first complete these tasks:

+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.

+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="iam-examples-managing-users-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the region for your code\. In this example, the region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'us-west-2'});
```

## Creating a User<a name="iam-examples-managing-users-creating-users"></a>

Create a Node\.js module with the file name `iam_createuser.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed, which consists of the user name you want to use for the new user as a command\-line parameter\.

Call the `getUser` method of the `AWS.IAM` service object to see if the user name already exists\. If the user name does not currently exist, call the `createUser` method to create it\. If the name already exists, write a message to that effect to the console\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  UserName: process.argv[2]
};

iam.getUser(params, function(err, data) {
  if (err && err.code === 'NoSuchEntity') {
    iam.createUser(params, function(err, data) {
      if (err) {
        throw err;
      } else {
        console.log("Success", data);
      }
    });
  } else {
    console.log("User " + process.argv[2] + " already exists", data.User.UserId);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_createuser.js USER_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_createuser.js)\.

## Listing Users in Your Account<a name="iam-examples-managing-users-listing-users"></a>

Create a Node\.js module with the file name `iam_listusers.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to list your users, limiting the number returned by setting the `MaxItems` parameter to 10\. Call the `listUsers` method of the `AWS.IAM` service object\. Write the first user's name and creation date to the console\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  MaxItems: 10
};

iam.listUsers(params, function(err, data) {
  if (err) {
    throw err;
  } else {
    var users = data.Users || [];
    users.forEach(function(user) {
      console.log("User " + user.UserName + " created", user.CreateDate);
    });
  }
});
```

To run the example, type the following at the command line\.

```
node iam_listusers.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_listusers.js)\.

## Updating a User's Name<a name="iam-examples-managing-users-updating-users"></a>

Create a Node\.js module with the file name `iam_updateuser.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to list your users, specifying both the current and new user names as command\-line parameters\. Call the `updateUser` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  UserName: process.argv[2],
  NewUserName: process.argv[3]
};

iam.updateUser(params, function(err, data) {
  if (err) {
    throw err;
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line, specifying the user's current name followed by the new user name\.

```
node iam_updateuser.js ORIGINAL_USERNAME NEW_USERNAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_updateuser.js)\.

## Deleting a User<a name="iam-examples-managing-users-deleting-users"></a>

Create a Node\.js module with the file name `iam_deleteuser.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed, which consists of the user name you want to delete as a command\-line parameter\.

Call the `getUser` method of the `AWS.IAM` service object to see if the user name already exists\. If the user name does not currently exist, write a message to that effect to the console\. If the user exists, call the `deleteUser` method to delete it\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  UserName: process.argv[2]
};

iam.getUser(params, function(err, data) {
  if (err && err.code === 'NoSuchEntity') {
    console.error("User " + process.argv[2] + " does not exist.");
    throw err;
  } else {
    iam.deleteUser(params, function(err, data) {
      if (err) {
        throw err;
      } else {
        console.log("User " + process.argv[2] + " deleted.");
      }
    });
  }
});
```

To run the example, type the following at the command line\.

```
node iam_deleteuser.js USER_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_deleteuser.js)\.