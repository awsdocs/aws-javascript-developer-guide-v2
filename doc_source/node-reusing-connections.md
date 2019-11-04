# Reusing Connections with Keep\-Alive in Node\.js<a name="node-reusing-connections"></a>

By default, the default Node\.js HTTP/HTTPS agent creates a new TCP connection for every new request\. To avoid the cost of establishing a new connection, you can reuse an existing connection\.

For short\-lived operations, such as DynamoDB queries, the latency overhead of setting up a TCP connection might be greater than the operation itself\. Additionally, since DynamoDB [encryption at rest](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html) is integrated with [AWS KMS](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html), you may experience latencies from the database having to re\-establish new AWS KMS cache entries for each operation\.

The easiest way to configure SDK for JavaScript to reuse TCP connections is to set the `AWS_NODEJS_CONNECTION_REUSE_ENABLED` environment variable to `1`\. This feature was added in the [2\.436\.0](https://github.com/aws/aws-sdk-js/blob/master/CHANGELOG.md#24630) release\.

Alternatively, you can set the `keepAlive` property of an HTTP or HTTPS agent set to `true`, as shown in the following example\.

```
const AWS = require('aws-sdk');
// http or https
const http = require('http');
const agent = new http.Agent({
  keepAlive: true
});

AWS.config.update({
  httpOptions: {
    agent
  }
});
```

The following example shows how to set `keepAlive` for just a DynamoDB client:

```
const AWS = require('aws-sdk')
// http or https
const https = require('https');
const agent = new https.Agent({
  keepAlive: true
});

const dynamodb = new AWS.DynamoDB({
  httpOptions: {
    agent
  }
});
```

If `keepAlive` is enabled, you can also set the initial delay for TCP Keep\-Alive packets with `keepAliveMsecs`, which by default is 1000ms\. See the [Node\.js documentation](https://nodejs.org/api/http.html) for details\.