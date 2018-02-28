# Getting Started in Node\.js<a name="getting-started-nodejs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**

+ How to create an Amazon Simple Storage Service \(Amazon S3\) service object\.

+ How to create an Amazon S3 bucket\.

+ How to upload an object to the created bucket\.

## Prerequisite Tasks<a name="getting-started-nodejs-prerequisites"></a>

To set up and run this example, you must first complete these tasks:

+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\. Downloads of the current and LTS versions of Node\.js for a variety of operating systems are found at [https://nodejs\.org/en/download/current/](https://nodejs.org/en/download/current/)\.

For more information on installing Node\.js packages, see [How to Install Local Packages](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) and [How to Create Node\.js Modules](https://docs.npmjs.com/getting-started/creating-node-modules) at the npm \(the Node\.js package manager\) website\. For information about downloading and installing the AWS SDK for JavaScript, see [Installing the SDK for JavaScript](installing-jssdk.md)\.


+ [Prerequisite Tasks](#getting-started-nodejs-prerequisites)
+ [Step 1: Downloading the Sample Project](#getting-started-nodejs-download)
+ [Step 2: Installing the SDK and Dependencies](#getting-started-nodejs-install-sdk)
+ [Step 3: Configuring the Access Keys](#getting-started-nodejs-configure-keys)
+ [Step 4: Running the Sample](#getting-started-nodejs-run-sample)

## Step 1: Downloading the Sample Project<a name="getting-started-nodejs-download"></a>

You can download the sample package from GitHub with the following command\. You must have Git installed\.

```
git clone https://github.com/awslabs/aws-nodejs-sample.git
```

## Step 2: Installing the SDK and Dependencies<a name="getting-started-nodejs-install-sdk"></a>

You install the SDK for JavaScript package using the [npm \(the Node\.js package manager\)](https://www.npmjs.com)\. From the `aws-nodejs-sample` directory in the package, type the following at the command line\.

```
npm install
```

## Step 3: Configuring the Access Keys<a name="getting-started-nodejs-configure-keys"></a>

You need to provide credentials to AWS so only your account and its resources are accessed by the SDK\. For more information about obtaining your account credentials, see [Getting Your Credentials](getting-your-credentials.md)\.

We recommend you create a shared credentials file to hold this information\. For more information about how to create a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\. Your credentials file should resemble the following example\.

```
[default]
aws_access_key_id = YOUR_ACCESS_KEY_ID
aws_secret_access_key = YOUR_SECRET_ACCESS_KEY
```

## Step 4: Running the Sample<a name="getting-started-nodejs-run-sample"></a>

Type the following command to run the sample\.

```
node sample.js
```