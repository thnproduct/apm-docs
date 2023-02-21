---
description: Switch where your Lambda runs like you switch lanes
---

# MerLoc - Debug & Hot-reload for Lambdas

**MerLoc** is a live AWS Lambda function development and debugging tool. MerLoc allows you to run AWS Lambda functions on your local while they are still part of a flow in the AWS cloud remote.

It currently supports Java, Go, Python, Node.js & .NET runtimes & works with serverless framework & AWS SAM under the hood.

![](https://4750167.fs1.hubspotusercontent-na1.net/hubfs/4750167/MerLoc/logos.png)

{% embed url="https://www.youtube.com/watch?v=4LIILFk1qAw&ab_channel=Thundra" %}

### Use-case example

Let's say that you have the following sample serverless architecture for your order application in your AWS account.&#x20;

And you are developing the `order-notification-service`.

With the help of MerLoc, you don’t need to

* deploy to test your function
* add debug log statements around code to debug your function
* re-deploy after every change to check and verify whether it fixes the bug
* run the function as standalone (without being part of the flow shown above) locally in Docker locally and prepare/provide the input manually

MerLoc makes it possible to

* test your function locally without deploy to the AWS Lambda environment (so no wait for build, package and deploy)
* debug your function by putting breakpoints from your IDE
* hot-reload updated function on your local automatically to apply changes automatically (so again no wait for build, package and deploy)
* run the individual function locally while it is still part of flow shown above and use real requests from the AWS Lambda environment

Additionally, MerLoc propagates IAM credentials from the real AWS Lambda environment to your local so your local function runs with the same credentials. So this means that you can also test and verify IAM permission issues on your local.

In the example shown above, when you run `order-notification-service` locally with MerLoc, `order-request-service` and `order-processing-service` will run on real AWS Lambda environment, and you will get real published message (by `order-processing-service`) from real `order-notification-topic` on your local `order-notification-service` and run as a part of real flow in the cloud.



### How MerLoc works?

![](https://4750167.fs1.hubspotusercontent-na1.net/hubfs/4750167/MerLoc/Merloc\_-Merloc-architecture.gif)

### Self-Hosted & SaaS Available

MerLoc is open-source and included with any package of Thundra APM. You can check [the MerLoc repository](https://github.com/thundra-io/merloc) and self-host your MerLoc broker.
