# Reusing connections with keep-alive in Node\.js<a name="node-reusing-connections"></a>

By default, Node\.js default HTTP/HTTPS agent creates a new TCP connection for every new request\. By not reusing already created connections, we incur the cost of establishing new ones, which includes 3-way or 7-way handshake depending on whether this is an HTTP or HTTPS transaction\. We are also not going to leverage the actual throughput established by the TCP protocol’s [flow and congestion controls](https://en.wikipedia.org/wiki/Additive_increase/multiplicative_decrease)\.

For short-lived operations completing within a single-digit ms (like DynamoDB queries) the latency overhead of setting up a TCP connection might be greater than the operation itself\. Additionally, DynamoDB encryption at rest is integrated with [AWS KMS](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html) and you may experience latencies from the database having to re-establish new AWS KMS cache entries for each operation ([read more](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html))\.

The easiest way to configure `aws-sdk-js` to reuse TCP connections is to set the `AWS_NODEJS_CONNECTION_REUSE_ENABLED` environment variable to `1`\. This feature was added in the [2.436.0](https://github.com/aws/aws-sdk-js/blob/master/CHANGELOG.md#24630) release\.

Alternatively, an HTTP or HTTPS agent can be supplied with `keepAlive` set to `true`\.

Example of setting it globally:

```js
const AWS = require('aws-sdk');

// http or https as per your use case
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

Example of setting `keepAlive` on a per-client basis:

```js
const AWS = require('aws-sdk')

// http or https as per your use case
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

If the `keepAlive` option is enabled, you can also set the initial delay for TCP Keep-Alive packets with `keepAliveMsecs`, which by default is 1000ms ([refer to Node\.js official docs for more details](https://nodejs.org/api/http.html))\.
