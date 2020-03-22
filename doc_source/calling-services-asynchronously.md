# Calling Services Asychronously<a name="calling-services-asynchronously"></a>

All requests made through the SDK are asynchronous\. This is important to keep in mind when writing browser scripts\. JavaScript running in a web browser typically has just a single execution thread\. After making an asynchronous call to an AWS service, the browser script continues running and in the process can try to execute code that depends on that asynchronous result before it returns\.

Making asynchronous calls to an AWS service includes managing those calls so your code doesn't try to use data before the data is available\. The topics in this section explain the need to manage asynchronous calls and detail different techniques you can use to manage them\.

**Topics**
+ [Managing Asychronous Calls](making-asynchronous-calls.md)
+ [Using an Anonymous Callback Function](using-a-callback-function.md)
+ [Using a Request Object Event Listener](using-a-response-event-handler.md)
+ [Using JavaScript Promises](using-promises.md)
+ [Using async/await](#using-async-await)
+ [Requests With a Node\.js Stream Object](requests-using-stream-objects.md)

## Using async/await<a name="using-async-await"></a>

An improvement on the normal method of using promises is async/await\. Async functions are simpler, take less boilerplate than using promises, and make the code read synchronous. When using await, it must be in a function marked as async and should be wrapped in a try/catch in order to catch any errors thrown. The await still needs a promise to "await" on, so you need to return a promise from the AWS.Request element using its promise() method.

```js
var s3 = new AWS.S3({apiVersion: '2006-03-01', region: 'us-west-2'});

async function putObject(params) {
  try { 
    // await on the S3 request promise
    var data = await s3.putObject(params).promise();
    console.log('Success');
  } catch (err) {
    console.log(err);
  }
);

var params = {
  Bucket: 'bucket',
  Key: 'example3.txt',
  Body: 'Uploaded text using the async/await method!'
};

putObject(params);
```
