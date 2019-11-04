# Using an Anonymous Callback Function<a name="using-a-callback-function"></a>

Each service object method that creates an `AWS.Request` object can accept an anonymous callback function as the last parameter\. The signature of this callback function is:

```
function(error, data) {
    // callback handling code
}
```

This callback function executes when either a successful response or error data returns\. If the method call succeeds, the contents of the response are available to the callback function in the `data` parameter\. If the call doesn't succeed, the details about the failure are provided in the `error` parameter\.

Typically the code inside the callback function tests for an error, which it processes if one is returned\. If an error is not returned, the code then retrieves the data in the response from the `data` parameter\. The basic form of the callback function looks like this example\.

```
function(error, data) {
    if (error) {
        // error handling code
        console.log(error);
    } else {
        // data handling code
        console.log(data);
    }
}
```

In the previous example, the details of either the error or the returned data are logged to the console\. Here is an example that shows a callback function passed as part of calling a method on a service object\.

```
new AWS.EC2({apiVersion: '2014-10-01'}).describeInstances(function(error, data) {
  if (error) {
    console.log(error); // an error occurred
  } else {
    console.log(data); // request succeeded
  }
});
```

## Accessing the Request and Response Objects<a name="access-request-response"></a>

Within the callback function, the JavaScript keyword `this` refers to the underlying `AWS.Response` object for most services\. In the following example, the `httpResponse` property of an `AWS.Response` object is used within a callback function to log the raw response data and headers to help with debugging\.

```
new AWS.EC2({apiVersion: '2014-10-01'}).describeInstances(function(error, data) {
  if (error) {
    console.log(error); // an error occurred
    // Using this keyword to access AWS.Response object and properties
    console.log("Response data and headers: " + JSON.stringify(this.httpResponse));
  } else {
    console.log(data); // request succeeded
  }
});
```

In addition, because the `AWS.Response` object has a `Request` property that contains the `AWS.Request` that was sent by the original method call, you can also access the details of the request that was made\.