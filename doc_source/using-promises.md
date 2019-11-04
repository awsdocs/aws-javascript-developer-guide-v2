# Using JavaScript Promises<a name="using-promises"></a>

The `AWS.Request.promise` method provides a way to call a service operation and manage asynchronous flow instead of using callbacks\. In Node\.js and browser scripts, an `AWS.Request` object is returned when a service operation is called without a callback function\. You can call the request's `send` method to make the service call\.

However, `AWS.Request.promise` immediately starts the service call and returns a promise that is either fulfilled with the response `data` property or rejected with the response `error` property\.

```
var request = new AWS.EC2({apiVersion: '2014-10-01'}).describeInstances();

// create the promise object
var promise = request.promise();

// handle promise's fulfilled/rejected states
promise.then(
  function(data) {
    /* process the data */
  },
  function(error) {
    /* handle the error */
  }
);
```

The next example returns a promise that's fulfilled with a `data` object, or rejected with an `error` object\. Using promises, a single callback isn't responsible for detecting errors\. Instead, the correct callback is called based on the success or failure of a request\.

```
var s3 = new AWS.S3({apiVersion: '2006-03-01', region: 'us-west-2'});
var params = {
  Bucket: 'bucket',
  Key: 'example2.txt',
  Body: 'Uploaded text using the promise-based method!'
};
var putObjectPromise = s3.putObject(params).promise();
putObjectPromise.then(function(data) {
  console.log('Success');
}).catch(function(err) {
  console.log(err);
});
```

## Coordinating Multiple Promises<a name="multiple-promises"></a>

In some situations, your code must make multiple asynchronous calls that require action only when they have all returned successfully\. If you manage those individual asynchronous method calls with promises, you can create an additional promise that uses the `all` method\. This method fulfills this umbrella promise if and when the array of promises that you pass into the method are fulfilled\. The callback function is passed an array of the values of the promises passed to the `all` method\.

In the following example, an AWS Lambda function must make three asynchronous calls to Amazon DynamoDB but can only complete after the promises for each call are fulfilled\.

```
Promise.all([firstPromise, secondPromise, thirdPromise]).then(function(values) {
  
  console.log("Value 0 is " + values[0].toString);
  console.log("Value 1 is " + values[1].toString);
  console.log("Value 2 is " + values[2].toString);

  // return the result to the caller of the Lambda function
  callback(null, values);
});
```

## Browser and Node\.js Support for Promises<a name="browser-node-promise-support"></a>

Support for native JavaScript promises \(ECMAScript 2015\) depends on the JavaScript engine and version in which your code executes\. To help determine the support for JavaScript promises in each environment where your code needs to run, see the [ECMAScript Compatability Table](https://kangax.github.io/compat-table/es6/) on GitHub\.

## Using Other Promise Implementations<a name="using-other-promise-implementations"></a>

In addition to the native promise implementation in ECMAScript 2015, you can also use third\-party promise libraries, including:
+ [bluebird](http://bluebirdjs.com)
+ [RSVP](https://github.com/tildeio/rsvp.js/)
+ [Q](https://github.com/kriskowal/q)

These optional promise libraries can be useful if you need your code to run in environments that don't support the native promise implementation in ECMAScript 5 and ECMAScript 2015\.

To use a third\-party promise library, set a promises dependency on the SDK by calling the `setPromisesDependency` method of the global configuration object\. In browser scripts, make sure to load the third\-party promise library before loading the SDK\. In the following example, the SDK is configured to use the implementation in the bluebird promise library\.

```
AWS.config.setPromisesDependency(require('bluebird'));
```

To return to using the native promise implementation of the JavaScript engine, call `setPromisesDependency` again, passing a `null` instead of a library name\.