# Requests With a Node\.js Stream Object<a name="requests-using-stream-objects"></a>

You can create a request that streams the returned data directly to a Node\.js `Stream` object by calling the `createReadStream` method on the request\. Calling `createReadStream` returns the raw HTTP stream managed by the request\. The raw data stream can then be piped into any Node\.js `Stream` object\.

This technique is useful for service calls that return raw data in their payload, such as calling `getObject` on an Amazon S3 service object to stream data directly into a file, as shown in this example\.

```
var s3 = new AWS.S3({apiVersion: '2006-03-01'});
var params = {Bucket: 'myBucket', Key: 'myImageFile.jpg'};
var file = require('fs').createWriteStream('/path/to/file.jpg');
s3.getObject(params).createReadStream().pipe(file);
```

When you stream data from a request using `createReadStream`, only the raw HTTP data is returned\. The SDK does not post\-process the data\.

Because Node\.js is unable to rewind most streams, if the request initially succeeds, then retry logic is disabled for the rest of the response\. In the event of a socket failure while streaming, the SDK won't attempt to retry or send more data to the stream\. Your application logic needs to identify such streaming failures and handle them\.