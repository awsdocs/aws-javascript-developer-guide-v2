--------

The AWS SDK for JavaScript version 3 \(v3\) is a rewrite of v2 with some great new features, including modular architecture\. For more information, see the [AWS SDK for JavaScript v3 Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)\.

--------

# Loading Credentials in Node\.js using a Configured Credential Process<a name="loading-node-credentials-configured-credential-process"></a>

You can source credentials by using a method that isn't built into the SDK\. To do this, specify a credential process in the shared AWS conﬁg file or the shared credentials file\. If the `AWS_SDK_LOAD_CONFIG` environment variable is set to a truthy value, the SDK will prefer the process specified in the config file over the process specified in the credentials file \(if any\)\.

For details about specifying a credential process in the shared AWS config file or the shared credentials file, see the *AWS CLI Command Reference*, specifically the information about [Sourcing Credentials From External Processes](https://docs.aws.amazon.com/cli/latest/topic/config-vars.html#sourcing-credentials-from-external-processes)\.

For information about using the `AWS_SDK_LOAD_CONFIG` environment variable, see [Using a Shared Config File](setting-region.md#setting-region-config-file) in this document\.