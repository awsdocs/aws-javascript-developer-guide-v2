# Loading Credentials in Node\.js from the Shared Credentials File<a name="loading-node-credentials-shared"></a>

You can keep your AWS credentials data in a shared file used by SDKs and the command line interface\. The SDK for JavaScript automatically searches the shared credentials file for credentials when loading\. Where you keep the shared credentials file depends on your operating system:
+ Linux, Unix, and macOS users: `~/.aws/credentials`
+ Windows users: `C:\Users\USER_NAME\.aws\credentials`

If you do not already have a shared credentials file, you can create one in the designated directory\. Add the following text to the credentials file, replacing *<YOUR\_ACCESS\_KEY\_ID>* and *<YOUR\_SECRET\_ACCESS\_KEY>* values:

```
[default]
aws_access_key_id = <YOUR_ACCESS_KEY_ID>
aws_secret_access_key = <YOUR_SECRET_ACCESS_KEY>
```

The `[default]` section heading specifies a default profile and associated values for credentials\. You can create additional profiles in the same shared configuration file, each with its own credential information\. The following example shows a configuration file with the default profile and two additional profiles:

```
[default] ; default profile
aws_access_key_id = <DEFAULT_ACCESS_KEY_ID>
aws_secret_access_key = <DEFAULT_SECRET_ACCESS_KEY>
    
[personal-account] ; personal account profile
aws_access_key_id = <PERSONAL_ACCESS_KEY_ID>
aws_secret_access_key = <PERSONAL_SECRET_ACCESS_KEY>
    
[work-account] ; work account profile
aws_access_key_id = <WORK_ACCESS_KEY_ID>
aws_secret_access_key = <WORK_SECRET_ACCESS_KEY>
```

By default, the SDK checks the `AWS_PROFILE` environment variable to determine which profile to use\. If the `AWS_PROFILE` variable is not set in your environment, the SDK uses the credentials for the `[default]` profile\. To use one of the additional profiles, change the value of the `AWS_PROFILE` environment variable\. In the previous example, to use the credentials from the work account, set `AWS_PROFILE=work-account`\.

After setting the environment variable, to run a `script.js` file that uses the SDK, type the following at the command line:

```
$ AWS_PROFILE=work-account node script.js
```

You can also explicitly select the profile used by the SDK, either by setting `process.env.AWS_PROFILE` before loading the SDK, or by selecting the credential provider as shown in the following example:

```
var credentials = new AWS.SharedIniFileCredentials({profile: 'work-account'});
AWS.config.credentials = credentials;
```