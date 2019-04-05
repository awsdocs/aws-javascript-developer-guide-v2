# Configuring Proxies for Node\.js<a name="node-configuring-proxies"></a>

If you can't directly connect to the internet, the SDK for JavaScript supports use of HTTP or HTTPS proxies through a third\-party HTTP agent, such as [proxy\-agent](https://github.com/TooTallNate/node-proxy-agent)\. To install proxy\-agent, type the following at the command line\.

```
npm install proxy-agent --save
```

If you decide to use a different proxy, first follow the installation and configuration instructions for that proxy\. To use this or another third\-party proxy in your application, you must set the `httpOptions` property of `AWS.Config` to specify the proxy you choose\. This example shows `proxy-agent`\. 

```
var proxy = require('proxy-agent');
AWS.config.update({
  httpOptions: { agent: proxy('http://internal.proxy.com') }
});
```

For more information about other proxy libraries, see [npm, the Node\.js package manager](https://www.npmjs.com/)\.