# Logging AWS SDK for JavaScript Calls<a name="logging-sdk-calls"></a>

The AWS SDK for JavaScript is instrumented with a built\-in logger so you can log API calls you make with the SDK for JavaScript\.

To turn on the logger and print log entries in the console, add the following statement to your code\.

```
AWS.config.logger = console;
```

Here is an example of the log output\.

```
[AWS s3 200 0.185s 0 retries] createMultipartUpload({ Bucket: 'js-sdk-test-bucket', Key: 'issues_1704' })
```

## Using a Third\-Party Logger<a name="third-party-logger"></a>

You can also use a third\-party logger, provided it has `log()` or `write()` operations to write to a log file or server\. You must install and set up your custom logger as instructed before you can use it with the SDK for JavaScript\.

One such logger you can use in either browser scripts or in Node\.js is cabin\. In Node\.js, you can configure cabin to write log entries to a log file\. You can also use it with webpack\.

When using a third\-party logger, set all options before assigning the logger to `AWS.config.logger`\. For example, the following specifies an external log file and sets the log level for logplease

```
// Require cabin
const Cabin = require('cabin');
const { Signale } = require('signale');
const pino = require('pino');

// Create a new logger instance
const logger = new Cabin({
  axe: {
    logger: process.env.NODE_ENV === 'production' ? pino({ customLevels: { log: 30 } }) : new Signale()
  }
});
// Assign logger to SDK
AWS.config.logger = logger;
```

For more information about cabin, see the [cabin JavaScript Logger](https://github.com/cabinjs/cabin) on GitHub\.
