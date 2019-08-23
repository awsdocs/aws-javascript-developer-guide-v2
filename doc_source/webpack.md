# Bundling Applications with Webpack<a name="webpack"></a>

Web applications in browser scripts or Node\.js use of code modules creates dependencies\. These code modules can have dependencies of their own, resulting in a collection of interconnected modules that your application requires to function\. To manage dependencies, you can use a module bundler like webpack\.

The webpack module bundler parses your application code, searching for `import` or `require` statements, to create bundles that contain all the assets your application needs so that the assets can be easily served through a webpage\. The SDK for JavaScript can be included in webpack as one of the dependencies to include in the output bundle\.

![\[Process by which webpack bundles application module dependencies into a static asset\]](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/images/webpack.png)

For more information about webpack, see the [webpack module bundler](https://webpack.github.io/) on GitHub\.

## Installing Webpack<a name="webpack-installing"></a>

To install the webpack module bundler, you must first have npm, the Node\.js package manager, installed\. Type the following command to install the webpack CLI and JavaScript module\.

```
npm install webpack
```

You may also need to install a webpack plugin that allows it to load JSON files\. Type the following command to install the JSON loader plugin\.

```
npm install json-loader
```

## Configuring Webpack<a name="webpack-configuring"></a>

By default, webpack searches for a JavaScript file named `webpack.config.js` in your project's root directory\. This file specifies your configuration options\. Here is an example of a `webpack.config.js` configuration file\.

```
// Import path for resolving file paths
var path = require('path');
module.exports = {
  // Specify the entry point for our app.
  entry: [
    path.join(__dirname, 'browser.js')
  ],
  // Specify the output file containing our bundled code
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
  module: {
    /**
      * Tell webpack how to load 'json' files. 
      * When webpack encounters a 'require()' statement
      * where a 'json' file is being imported, it will use
      * the json-loader.  
      */
    loaders: [
      {
        test: /\.json$/, 
        loaders: ['json']
      }
    ]
  }
}
```

In this example, `browser.js` is specified as the entry point\. The *entry point* is the file webpack uses to begin searching for imported modules\. The file name of the output is specified as `bundle.js`\. This output file will contain all the JavaScript the application needs to run\. If the code specified in the entry point imports or requires other modules, such as the SDK for JavaScript, that code is bundled without needing to specify it in the configuration\.

The configuration in the `json-loader` plugin that was installed earlier specifies to webpack how to import JSON files\. By default, webpack only supports JavaScript but uses loaders to add support for importing other file types\. Because the SDK for JavaScript makes extensive use of JSON files, webpack throws an error when generating the bundle if `json-loader` isn't included\.

## Running Webpack<a name="webpack-running"></a>

To build an application to use webpack, add the following to the `scripts` object in your `package.json` file\.

```
"build": "webpack"
```

Here is an example `package.json` that demonstrates adding webpack\.

```
{
  "name": "aws-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "aws-sdk": "^2.6.1"
  },
  "devDependencies": {
    "json-loader": "^0.5.4",
    "webpack": "^1.13.2"
  }
}
```

To build your application, type the following command\.

```
npm run build
```

The webpack module bundler then generates the JavaScript file you specified in your project's root directory\.

## Using the Webpack Bundle<a name="webpack-using-bundle"></a>

To use the bundle in a browser script, you can incorporate the bundle using a `<script>` tag as shown in the following example\.

```
<!DOCTYPE html>
<html>
    <head>
        <title>AWS SDK with webpack</title>
    </head> 
    <body>
        <div id="list"></div>
        <script src="bundle.js"></script>
    </body>
</html>
```

## Importing Individual Services<a name="webpack-importing-services"></a>

One of the benefits of webpack is that it parses the dependencies in your code and bundles only the code your application needs\. If you are using the SDK for JavaScript, bundling only the parts of the SDK actually used by your application can reduce the size of the webpack output considerably\.

Consider the following example of the code used to create an Amazon S3 service object\.

```
// Import the AWS SDK
var AWS = require('aws-sdk');

// Set credentials and Region
// This can also be done directly on the service client
AWS.config.update({region: 'us-west-1', credentials: {YOUR_CREDENTIALS}});

var s3 = new AWS.S3({apiVersion: '2006-03-01'});
```

The `require()` function specifies the entire SDK\. A webpack bundle generated with this code would include the full SDK but the full SDK is not required when only the Amazon S3 client class is used\. The size of the bundle would be substantially smaller if only the portion of the SDK you require for the Amazon S3 service was included\. Even setting the configuration doesn't require the full SDK because you can set the configuration data on the Amazon S3 service object\.

Here is what the same code looks like when it includes only the Amazon S3 portion of the SDK\.

```
// Import the Amazon S3 service client
var S3 = require('aws-sdk/clients/s3');
 
// Set credentials and Region
var s3 = new S3({
    apiVersion: '2006-03-01',
    region: 'us-west-1', 
    credentials: {YOUR_CREDENTIALS}
  });
```

## Bundling for Node\.js<a name="webpack-nodejs-bundles"></a>

You can use webpack to generate bundles that run in Node\.js by specifying it as a target in the configuration\.

```
target: "node"
```

This is useful when running a Node\.js application in an environment where disk space is limited\. Here is an example `webpack.config.js` configuration with Node\.js specified as the output target\.

```
// Import path for resolving file paths
var path = require('path');
module.exports = {
  // Specify the entry point for our app
  entry: [
    path.join(__dirname, 'node.js')
  ],
  // Specify the output file containing our bundled code
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
  // Let webpack know to generate a Node.js bundle
  target: "node",
  module: {
    /**
      * Tell webpack how to load JSON files.
      * When webpack encounters a 'require()' statement
      * where a JSON file is being imported, it will use
      * the json-loader
      */
    loaders: [
      {
        test: /\.json$/, 
        loaders: ['json']
      }
    ]
  }
}
```