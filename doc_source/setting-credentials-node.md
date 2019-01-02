# Setting Credentials in Node\.js<a name="setting-credentials-node"></a>

There are several ways in Node\.js to supply your credentials to the SDK\. Some of these are more secure and others afford greater convenience while developing an application\. When obtaining credentials in Node\.js, be careful about relying on more than one source such as an environment variable and a JSON file you load\. You can change the permissions under which your code runs without realizing the change has happened\.

Here are the ways you can supply your credentials in order of recommendation:

1. Loaded from AWS Identity and Access Management \(IAM\) roles for Amazon EC2 \(if running on Amazon EC2\)

1. Loaded from the shared credentials file \(`~/.aws/credentials`\)

1. Loaded from environment variables

1. Loaded from a JSON file on disk

**Warning**  
While it is possible to do so, we do not recommend hard\-coding your AWS credentials in your application\. Hard\-coding credentials poses a risk of exposing your access key ID and secret access key\.

The topics in this section describe how to load credentials into Node\.js\.

**Topics**
+ [Loading Credentials in Node\.js from IAM Roles for EC2](loading-node-credentials-iam.md)
+ [Loading Credentials for a Node\.js Lambda Function](loading-node-credentials-lambda.md)
+ [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)
+ [Loading Credentials in Node\.js from Environment Variables](loading-node-credentials-environment.md)
+ [Loading Credentials in Node\.js from a JSON File](loading-node-credentials-json-file.md)