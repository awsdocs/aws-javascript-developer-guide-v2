# Using Async/Await<a name="using-async-await"></a>

Async/Await is a better and more modern way of using promises. Async functions are simpler, take less boilerplate than using promises, and make the code look synchronous. When using await, it must be in a function marked as async and should be wrapped in a try/catch in order to catch any errors thrown. The await still needs a promise to "await" on, so you need to return a promise from the AWS.Request element using its promise() method.

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
