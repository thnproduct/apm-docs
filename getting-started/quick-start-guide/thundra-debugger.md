---
description: AWS Lambda Debugger is now called Serverless Debugger!
---

# AWS Lambda Debugger

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

Serverless applications come with many advantages, such as a quicker time to release, automatic scaling of applications, and flexibility. However, when users migrate to serverless, they discover the challenges that come from the result of old developer habits no longer being available. One of the strongest challenges is debugging. Debugging allows developers to find the origin of any issue and solve it. On a daily basis, developers need to debug their applications on their local IDE, but this isn’t possible in serverless applications. That’s where Thundra comes in. Thundra Debugger eases this pain by offering a remote debugging experience on your local IDE.

### How to get Authentication Key for AWS Lambda Debugger

You need to build a connection through an authentication key between the Lambda function and your local IDE to debug your Lambda function. This authentication is established with the Thundra Authentication Key. &#x20;

You can find your authentication key on the [Thundra AWS Lambda Debugger panel](https://start.thundra.io/apps#showDebugToken)

![You can gather your AWS Lambda Debugger key by clicking the link on the first card](<../../.gitbook/assets/Screen Shot 2022-01-20 at 17.17.49.png>)



We currently support Java, Python, and Node.js runtimes for debugging. Follow the instructions to have a native debugging experience for your Lambda functions:

* [Node.js](../../node.js/online-debugging.md)
* [Python](../../python/online-debugging.md)
* [Java](../../java/online-debugging.md)

To ease your debugging experience, VSCode and IntelliJ Idea plugins are available on marketplaces:

* [VSCode Plugin](../../ide-integrations/vscode-plugin/)
* [IntelliJ IDEA Plugin](../../ide-integrations/intellij-plugin.md)
