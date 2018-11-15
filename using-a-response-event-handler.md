# Using a Request Object Event Listener<a name="using-a-response-event-handler"></a>

If you do not create and pass an anonymous callback function as a parameter when you call a service object method, the method call generates an `AWS.Request` object that must be manually sent using its `send` method\.

To process the response, you must create an event listener for the `AWS.Request` object to register a callback function for the method call\. The following example shows how to create the `AWS.Request` object for calling a service object method and the event listener for the successful return\.

```
// create the AWS.Request object
var request = new AWS.EC2({apiVersion: '2014-10-01'}).describeInstances();

// register a callback event handler
request.on('success', function(response) {
  // log the successful data response
  console.log(response.data); 
});

// send the request
request.send();
```

After the `send` method on the `AWS.Request` object is called, the event handler executes when the service object receives an `AWS.Response` object\.

For more information about the `AWS.Request` object, see [Class: AWS\.Request](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Request.html) in the API Reference\. For more information about the `AWS.Response` object, see [Using the Response Object](the-response-object.md) or [Class: AWS\.Response](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Response.html) in the API Reference\.

## Chaining Multiple Callbacks<a name="response-chaining-callbacks"></a>

You can register multiple callbacks on any request object\. Multiple callbacks can be registered for different events or the same event\. Also, you can chain callbacks as shown in the following example\.

```
request.
  on('success', function(response) {
    console.log("Success!");
  }).
  on('error', function(response) {
    console.log("Error!");
  }).
  on('complete', function() {
    console.log("Always!");
  }).
  send();
```

## Request Object Completion Events<a name="request-object-completion-events"></a>

The `AWS.Request` object raises these completion events based on the response of each service operation method:
+ `success`
+ `error`
+ `complete`

You can register a callback function in response to any of these events\. For a complete list of all request object events, see [Class: AWS\.Request](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Request.html) in the API Reference\.

### The success Event<a name="request-success-event"></a>

The `success` event is raised upon a successful response received from the service object\. Here is how you register a callback function for this event\.

```
request.on('success', function(response) { 
  // event handler code
});
```

The response provides a `data` property that contains the serialized response data from the service\. For example, the following call to the `listBuckets` method of the Amazon S3 service object

```
s3.listBuckets.on('success', function(response) {
  console.log(response.data);
}).send();
```

returns the response and then prints the following `data` property contents to the console\.

```
{ Owner: { ID: '...', DisplayName: '...' },
  Buckets: 
   [ { Name: 'someBucketName', CreationDate: someCreationDate },
     { Name: 'otherBucketName', CreationDate: otherCreationDate } ],
  RequestId: '...' }
```

### The error Event<a name="request-error-event"></a>

The `error` event is raised upon an error response received from the service object\. Here is how you register a callback function for this event\.

```
request.on('error', function(error, response) { 
  // event handling code
});
```

When the `error` event is raised, the value of the response's `data` property is `null` and the `error` property contains the error data\. The associated `error` object is passed as the first parameter to the registered callback function\. For example, the following code:

```
s3.config.credentials.accessKeyId = 'invalid';
s3.listBuckets().on('error', function(error, response) {
  console.log(error);
}).send();
```

returns the error and then prints the following error data to the console\.

```
{ code: 'Forbidden', message: null }
```

### The complete Event<a name="request-complete-event"></a>

The `complete` event is raised when a service object call has finished, regardless of whether the call results in success or error\. Here is how you register a callback function for this event\.

```
request.on('complete', function(response) { 
  // event handler code
});
```

Use the `complete` event callback to handle any request cleanup that must execute regardless of success or error\. If you use response data inside a callback for the `complete` event, first check the `response.data` or `response.error` properties before attempting to access either one, as shown in the following example\.

```
request.on('complete', function(response) {
  if (response.error) {
    // an error occurred, handle it
  } else {
    // we can use response.data here
  }
}).send();
```

## Request Object HTTP Events<a name="request-object-http-events"></a>

The `AWS.Request` object raises these HTTP events based on the response of each service operation method:
+ `httpHeaders`
+ `httpData`
+ `httpUploadProgress`
+ `httpDownloadProgress`
+ `httpError`
+ `httpDone`

You can register a callback function in response to any of these events\. For a complete list of all request object events, see [Class: AWS\.Request](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Request.html) in the API Reference\.

### The httpHeaders Event<a name="request-httpheaders-event"></a>

The `httpHeaders` event is raised when headers are sent by the remote server\. Here is how you register a callback function for this event\.

```
request.on('httpHeaders', function(statusCode, headers, response) {
  // event handling code
});
```

The `statusCode` parameter to the callback function is the HTTP status code\. The `headers` parameter contains the response headers\.

### The httpData Event<a name="request-httpdata-event"></a>

The `httpData` event is raised to stream response data packets from the service\. Here is how you register a callback function for this event\.

```
request.on('httpData', function(chunk, response) {
  // event handling code
});
```

This event is typically used to receive large responses in chunks when loading the entire response into memory is not practical\. This event has an additional `chunk` parameter that contains a portion of the actual data from the server\.

If you register a callback for the `httpData` event, the `data` property of the response contains the entire serialized output for the request\. You must remove the default `httpData` listener if you don't have the extra parsing and memory overhead for the built\-in handlers\.

### The httpUploadProgress and httpDownloadProgress Events<a name="request-httpupload-download-progress-event"></a>

The `httpUploadProgress` event is raised when the HTTP request has uploaded more data\. Similarly, the `httpDownloadProgress` event is raised when the HTTP request has downloaded more data\. Here is how you register a callback function for these events\.

```
request.on('httpUploadProgress', function(progress, response) {
  // event handling code
})
.on('httpDownloadProgress', function(progress, response) {
  // event handling code
});
```

The `progress` parameter to the callback function contains an object with the loaded and total bytes of the request\.

### The httpError Event<a name="request-httperror-event"></a>

The `httpError` event is raised when the HTTP request fails\. Here is how you register a callback function for this event\.

```
request.on('httpError', function(error, response) {
  // event handling code
});
```

The `error` parameter to the callback function contains the error that was thrown\.

### The httpDone Event<a name="request-httpdone-event"></a>

The `httpDone` event is raised when the server finishes sending data\. Here is how you register a callback function for this event\.

```
request.on('httpDone', function(response) {
  // event handling code
});
```