--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Loading Credentials in Node\.js from IAM roles for Amazon EC2<a name="loading-node-credentials-iam"></a>

If you run your Node\.js application on an Amazon EC2 instance, you can leverage IAM roles for Amazon EC2 to automatically provide credentials to the instance\. If you configure your instance to use IAM roles, the SDK automatically selects the IAM credentials for your application, eliminating the need to manually provide credentials\.

For more information on adding IAM roles to an Amazon EC2 instance, see [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)\.