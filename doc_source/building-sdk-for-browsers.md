# Building the SDK for Browsers<a name="building-sdk-for-browsers"></a>

The SDK for JavaScript is provided as a JavaScript file with support included for a default set of services\. This file is typically loaded into browser scripts using a `<script>` tag that references the hosted SDK package\. However, you may need support for services other than the default set or otherwise need to customize the SDK\.

If you work with the SDK outside of an environment that enforces CORS in your browser and if you want access to all services provided by the SDK for JavaScript, you can build a custom copy of the SDK locally by cloning the repository and running the same build tools that build the default hosted version of the SDK\. The following sections describe the steps to build the SDK with extra services and API versions\.

**Topics**
+ [Using the SDK Builder to Build the SDK for JavaScript](#using-the-sdk-builder)
+ [Using the CLI to Build the SDK for JavaScript](#using-command-line-tools)
+ [Building Specific Services and API Versions](#building-specific-services-versions)
+ [Building the SDK as a Dependency with Browserify](#building-using-browserify)

## Using the SDK Builder to Build the SDK for JavaScript<a name="using-the-sdk-builder"></a>

The easiest way to create your own build of the AWS SDK for JavaScript is to use the SDK builder web application at [https://sdk\.amazonaws\.com/builder/js](https://sdk.amazonaws.com/builder/js)\. Use the SDK builder to specify services, and their API versions, to include in your build\. 

![\[SDK builder web interface for customizing SDK builds\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/sdk-builder.png)

Choose **Select all services** or choose **Select default services** as a starting point from which you can add or remove services\. Choose **Development** for more readable code or choose **Minified** to create a minified build to deploy\. After you choose the services and versions to include, choose **Build** to build and download your custom SDK\.

## Using the CLI to Build the SDK for JavaScript<a name="using-command-line-tools"></a>

To build the SDK for JavaScript using the AWS CLI, you first need to clone the Git repository that contains the SDK source\. You must have Git and Node\.js installed on your computer\.

First, clone the repository from GitHub and change directory into the directory:

```
git clone git://github.com/aws/aws-sdk-js
cd aws-sdk-js
```

After you clone the repository, download the dependency modules for both the SDK and build tool:

```
npm install
```

You can now build a packaged version of the SDK\.

### Building from the Command Line<a name="building-from-command-line"></a>

The builder tool is in `dist-tools/browser-builder.js`\. Run this script by typing:

```
node dist-tools/browser-builder.js > aws-sdk.js
```

This command builds the aws\-sdk\.js file\. This file is uncompressed\. By default this package includes only the standard set of services\. 

### Minifying Build Output<a name="minifying-build-output"></a>

To reduce the amount of data on the network, JavaScript files can be compressed through a process called *minification*\. Minification strips comments, unnecessary spaces, and other characters that aid in human readability but that do not impact execution of the code\. The builder tool can produce uncompressed or minified output\. To minify your build output, set the `MINIFY` environment variable:

```
MINIFY=1 node dist-tools/browser-builder.js > aws-sdk.js
```

## Building Specific Services and API Versions<a name="building-specific-services-versions"></a>

You can select which services to build into the SDK\. To select services, specify the service names, delimited by commas, as parameters\. For example, to build only Amazon S3 and Amazon EC2, use the following command:

```
node dist-tools/browser-builder.js s3,ec2 > aws-sdk-s3-ec2.js
```

You can also select specific API versions of the services build by adding the version name after the service name\. For example, to build both API versions of Amazon DynamoDB, use the following command:

```
node dist-tools/browser-builder.js dynamodb-2011-12-05,dynamodb-2012-08-10
```

Service identifiers and API versions are available in the service\-specific configuration files at [https://github\.com/aws/aws\-sdk\-js/tree/master/apis](https://github.com/aws/aws-sdk-js/tree/master/apis)\.

### Building All Services<a name="building-all-services"></a>

You can build all services and API versions by including the `all` parameter:

```
node dist-tools/browser-builder.js all > aws-sdk-full.js
```

### Building Specific Services<a name="building-specific-services"></a>

To customize the selected set of services included in the build, pass the `AWS_SERVICES` environment variable to the Browserify command that contains the list of services you want\. The following example builds the Amazon EC2, Amazon S3, and DynamoDB services\.

```
$ AWS_SERVICES=ec2,s3,dynamodb browserify index.js > browser-app.js
```

## Building the SDK as a Dependency with Browserify<a name="building-using-browserify"></a>

Node\.js has a module\-based mechanism for including code and functionality from third\-party developers\. This modular approach is not natively supported by JavaScript running in web browsers\. However, with a tool called Browserify, you can use the Node\.js module approach and use modules written for Node\.js in the browser\. Browserify builds the module dependencies for a browser script into a single, self\-contained JavaScript file that you can use in the browser\.

You can build the SDK as a library dependency for any browser script by using Browserify\. For example, the following Node\.js code requires the SDK:

```
var AWS = require('aws-sdk');
var s3 = new AWS.S3();
s3.listBuckets(function(err, data) { console.log(err, data); });
```

This example code can be compiled into a browser\-compatible version using Browserify:

```
$ browserify index.js > browser-app.js
```

The application, including its SDK dependencies, is then made available in the browser through `browser-app.js`\.

For more information about Browserify, see the [Browserify website](http://browserify.org/)\.