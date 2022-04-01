--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Registering Certificate Bundles in Node\.js<a name="node-registering-certs"></a>

The default trust stores for Node\.js include the certificates needed to access AWS services\. In some cases, it might be preferable to include only a specific set of certificates\.

In this example, a specific certificate on disk is used to create an `https.Agent` that rejects connections unless the designated certificate is provided\. The newly created `https.Agent` is then used to update the SDK configuration\.

```
var fs = require('fs');
var https = require('https');
var certs = [
  fs.readFileSync('/path/to/cert.pem')
];
    
AWS.config.update({
  httpOptions: {
    agent: new https.Agent({
      rejectUnauthorized: true,
      ca: certs
    })
  }
});
```