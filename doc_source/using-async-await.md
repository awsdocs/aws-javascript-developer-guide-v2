--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Using async/await<a name="using-async-await"></a>

You can use the `async/await` pattern in your calls to the AWS SDK for JavaScript\. Most functions that take a callback do not return a promise\. Since you only use `await` functions that return a promise, to use the `async/await` pattern you need to chain the `.promise()` method to the end of your call, and remove the callback\.

The following example uses async/await to list all of your Amazon DynamoDB tables in `us-west-2`\.

```
var AWS = require("aws-sdk");
//Create an Amazon DynamoDB client service object.
dbClient = new AWS.DynamoDB({ region: "us-west-2" });
// Call DynamoDB to list existing tables
const run = async () => {
  try {
    const results = await dbClient.listTables({}).promise();
    console.log(results.TableNames.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
run();
```

**Note**  
 Not all browsers support async/await\. See [Async functions](https://caniuse.com/#feat=async-functions) for a list of browsers with async/await support\. 