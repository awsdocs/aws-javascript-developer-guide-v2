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

Rather than using promises, you should consider using async/await\. Async functions are simpler and take less boilerplate than using promises\. Await can only be used in an async function to asynchronously wait for a value\.