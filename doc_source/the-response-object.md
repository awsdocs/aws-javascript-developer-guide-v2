# Using the Response Object<a name="the-response-object"></a>

After a service object method has been called, it returns an `AWS.Response` object by passing it to your callback function\. You access the contents of the response through the properties of the `AWS.Response` object\. There are two properties of the `AWS.Response` object you use to access the contents of the response:
+ `data` property
+ `error` property

When using the standard callback mechanism, these two properties are provided as parameters on the anonymous callback function as shown in the following example\.

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

## Accessing Data Returned in the Response Object<a name="response-data-property"></a>

The `data` property of the `AWS.Response` object contains the serialized data returned by the service request\. When the request is successful, the `data` property contains an object that contains a map to the data returned\. The `data` property can be null if an error occurs\.

Here is an example of calling the `getItem` method of a DynamoDB table to retrieve the file name of an image file to use as part of a game\.

```
// Initialize parameters needed to call DynamoDB
var slotParams = {
    Key : {'slotPosition' : {N: '0'}},
    TableName : 'slotWheels',
    ProjectionExpression: 'imageFile'
};

// prepare request object for call to DynamoDB
var request = new AWS.DynamoDB({region: 'us-west-2', apiVersion: '2012-08-10'}).getItem(slotParams);
// log the name of the image file to load in the slot machine
request.on('success', function(response) {
    // logs a value like "cherries.jpg" returned from DynamoDB
    console.log(response.data.Item.imageFile.S);
});
// submit DynamoDB request
request.send();
```

For this example, the DynamoDB table is a lookup of images that show the results of a slot machine pull as specified by the parameters in `slotParams`\.

Upon a successful call of the `getItem` method, the `data` property of the `AWS.Response` object contains an `Item` object returned by DynamoDB\. The returned data is accessed according to the request's `ProjectionExpression` parameter, which in this case means the `imageFile` member of the `Item` object\. Because the `imageFile` member holds a string value, you access the file name of the image itself through the value of the `S` child member of `imageFile`\.

## Paging Through Returned Data<a name="response-paged-data"></a>

Sometimes the contents of the `data` property returned by a service request span multiple pages\. You can access the next page of data by calling the `response.nextPage` method\. This method sends a new request\. The response from the request can be captured either with a callback or with success and error listeners\.

You can check to see if the data returned by a service request has additional pages of data by calling the `response.hasNextPage` method\. This method returns a boolean to indicate whether calling `response.nextPage` returns additional data\.

```
s3.listObjects({Bucket: 'bucket'}).on('success', function handlePage(response) {
    // do something with response.data
    if (response.hasNextPage()) {
        response.nextPage().on('success', handlePage).send();
    }
}).send();
```

## Accessing Error Information from a Response Object<a name="response-error-property"></a>

The `error` property of the `AWS.Response` object contains the available error data in the event of a service error or transfer error\. The error returned takes the following form\.

```
{ code: 'SHORT_UNIQUE_ERROR_CODE', message: 'a descriptive error message' }
```

In the case of an error, the value of the `data` property is `null`\. If you handle events that can be in a failure state, always check whether the `error` property was set before attempting to access the value of the `data` property\.

## Accessing the Originating Request Object<a name="response-request-property"></a>

The `request` property provides access to the originating `AWS.Request` object\. It can be useful to refer to the original `AWS.Request` object to access the original parameters it sent\. In the following example, the `request` property is used to access the `Key` parameter of the original service request\.

```
s3.getObject({Bucket: 'bucket', Key: 'key'}).on('success', function(response) {
   console.log("Key was", response.request.params.Key);
}).send();
```