# Step by Step Migration Guide from Epsagon to Thundra

We as Thundra have been serving serverless users since 2018. NodeJS, Java, Python, .Net, and Go applications met effortless monitoring with our technology. Cloud vendors like AWS, Microsoft, Google supported the fantastic serverless community and we have been proud to be a part of it as a third-party monitoring and debugging solution.

Along the way, we shaped our roadmap with our customers and created three foundational developer tools. (_Epsagon is a substitute for Thundra APM only_)

* [Thundra APM](https://www.thundra.io/apm): Serverless and container monitoring tool.
* [Thundra Sidekick](https://www.thundra.io/sidekick): Remote and non-intrusive debugging tool.
* [Thundra Foresight](https://www.thundra.io/foresight): CI monitoring and testing Software

\----------

In this article, since our esteemed competitor Epsagon has been acquired by Cisco, we are showing the simplest way to migrate to Thundra for their customers.

1. [How to remove Epsagon from your stack](step-by-step-migration-guide-from-epsagon-to-thundra.md#sub\_1)
2. [How to install Thundra APM to your stack](step-by-step-migration-guide-from-epsagon-to-thundra.md#sub\_2)
3. [Extra benefits](step-by-step-migration-guide-from-epsagon-to-thundra.md#sub\_3)

\----------





## How to remove Epsagon from your stack <a href="#sub_1" id="sub_1"></a>

Depending on how you integrated Epsagon into your stack, there may be several methods for removal.

### Using Serverless plugin

You can uninstall it as follows:

_`serverless plugin uninstall --name serverless-epsagon-layers`_

This should both uninstall the plugin and remove it from plugins in serverless.yml.

You can remove Epsagon in the custom section of serverless.yml to remove it completely.

### Using layers

Without the Serverless plugin, assuming you have done integration by changing your functions handlers to Epsagon wrappers and put Epsagon layers to your function configurations:

_Reset handlers of your functions and remove Epsagon layer configurations._

### Using manual wrappers

If you have used manual instrumentation, you can find substitute mechanisms for manual instrumentation that are easily applied to Thundra.

### Using CloudWatch integration

If you have enabled CloudWatch log collecting, you can remove it by navigating to Epsagon Teams Page, choosing a team, and deleting the AWS role.

Also, you can remove the stack you possibly created before for this role from the AWS console.

## How to install Thundra APM to your stack <a href="#sub_2" id="sub_2"></a>

Thundra APM is designed with plug and play mentality. There are three simple steps to get started and onboard with Thundra APM.

1. _**Create a Thundra account**_
2. _**Connect Thundra to your stack**_
3. _**Instrument agents for deeper observability**_

### Creating a Thundra account

All you have to do is visit [https://start.thundra.io/](https://start.thundra.io/) and sign up for Thundra.

![](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-53-67-PM.png?width=624\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-53-67-PM.png)

_Image 1 - Thundra’s Product Selection Screen_

Since Thundra has three products, you need to choose the APM product to carry on.

### Connecting Thundra to your stack

After you create your Thundra account, you will be able to access the Thundra application.

At this point, you can click on the “here” text and see the instructions, or, you can click on the rocket sign from the sidebar to start connecting your application.

**Selecting your data source**\
There are 2 options to select a data source to get started with Thundra APM. You can connect your AWS account to manually instrument your application.

Thundra APM does not obligate you to connect your AWS account but you can also manually instrument your application. You can see detailed information [here](https://apm.docs.thundra.io/getting-started/quick-start-guide/connect-thundra).

**Connect your AWS account**\
Step 1: You can connect your AWS account and add our CloudFormation stack into your AWS easily. In order to do that click “Connect Thundra'', as shown below.

![Image 2 - Connect your AWS account to Thundra APM](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-56-27-PM.png?width=624\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-56-27-PM.png)

Step2: You will be led to a page informing you about what needs to be done when you click the “Connect Thundra” button below. Make sure that your AWS is ready to connect.

![Image 3 - Create AWS ​​CloudFormation stack](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-57-77-PM.png?width=624\&height=344\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-57-77-PM.png)

Thundra will help you create a CloudFormation stack when you click on the “Connect Thundra'' button. When creation is finished, you are able to display a list of your functions.

Step 3: You can select functions that you want to monitor on Thundra from the list. You may have a lot of functions, don't worry. You can always alter subscriptions of your Lambda functions from the AWS tab on the settings page.

![Image 4 - Create AWS ​​CloudFormation stack - Success!](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-56-61-PM.png?width=624\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-56-61-PM.png)

#### Instrumenting for deeper observability

You have three options to instrument your code with Thundra APM.

* [Manual instrumentation](https://apm.docs.thundra.io/getting-started/quick-start-guide/instrument-to-achieve-deeper-level-of-visibility#manual-instrumentation)
* [Automated instrumentation](https://apm.docs.thundra.io/getting-started/quick-start-guide/instrument-to-achieve-deeper-level-of-visibility#automated-instrumentation)
* [Instrumentation through the Thundra console](https://apm.docs.thundra.io/thundra-web-console/functions-list-page#instrumentation)

Detailed information about automated and manual instrumentation can be found on the given links.

Instrumentation through the console:

You can easily instrument your functions directly from the console. To do this, you have to connect your AWS account to Thundra from [here](https://console.thundra.io/settings/aws).

Note that Thundra updates your function configuration in your AWS account with this option. The steps that are automatically conducted are:

* Adding Thundra Layer to your function.
* Changing the runtime of your function to a custom runtime provided by Thundra (for Node.js, .NET, and Java).
* Assigning a Thundra API key.

You can get more information about how to do this [here](https://apm.docs.thundra.io/thundra-web-console/functions-list-page#instrumentation).

## Extra benefits <a href="#sub_3" id="sub_3"></a>

Anyone who is new to Thundra may wonder about the key features and benefits. In this last section of the article, I'm going to list some useful features of Thundra APM.

#### Thundra APM supports your stack

Thundra APM supports your stack! It works everywhere! Its lightweight agents provide automated instrumentation and distributed tracing for your applications on containers, VMs, serverless and more. It does not require any manual work, additional code, training or maintenance.

![](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-57-04-PM.png?width=624\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-57-04-PM.png)

_Image 5 - Distributed tracing_

#### Distributed transactions are visualized automatically

Thundra APM visualizes every transaction with automated distributed tracing allowing you to understand the flow and correlate issues across various services.

You can trace [distributed transactions](https://apm.docs.thundra.io/monitoring/tracking-serverless-transactions-end-to-end) end-to-end and understand the root causes of the errors and latencies of your serverless and containerized applications by automated tracing.

![](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-57-47-PM.png?width=624\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-57-47-PM.png)

_Image 6 - Distributed tracing_

#### Time Travel Debugging (AKA Record and Replay)

You don’t need to reproduce errors, or latencies in your local environment when you use Thundra APM. You can easily debug your code and discover error root causes using time travel debugging.

Thundra APM’s [time travel debugging](https://apm.docs.thundra.io/debugging/offline-debugging) feature enables you to step over each code line of your application after the execution and track the values of the local variables. It is just like replaying the execution exactly in the same cloud environment. You can debug your event-driven architectures and uncover the unknowns.

![](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-55-86-PM.png?width=624\&height=584\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-Nov-03-2021-12-51-55-86-PM.png)

_Image 7 - Time travel debugging_

#### Real-Time (Live) Serverless Debugging

Thundra’s [online debugger](https://apm.docs.thundra.io/debugging/online-debugging) runs on your cloud environment while all the other solutions simulate it. Thundra allows you to debug your serverless applications on the cloud with their own permissions. Thundra’s online debugger sets up a secure bridge between your AWS Lambda environment and your IDE. [VSCode](https://marketplace.visualstudio.com/items?itemName=thundra.thundra-debugger\&ssr=false) and [IntelliJ IDEA](https://plugins.jetbrains.com/plugin/13858-debugger-for-aws-lambda) are natively supported with plugins. For other IDEs, we provide a portable client to foster the integration with any IDEs.

![](https://blog.thundra.io/hs-fs/hubfs/Google%20Drive%20Integration/Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-2.gif?width=624\&height=376\&name=Step%20by%20Step%20Migration%20Guide%20from%20Epsagon%20to%20Thundra-2.gif)

_Image 8 - Real-time serverless debugging_

#### Chaos Engineering

With Thundra APM, you can test the failure scenarios by injecting chaos into your applications. This feature allows you to inject errors to your Lambda functions to simulate failures in your serverless system. Thundra currently supports Chaos Injection in Java, Node.js, and Python.

For an example usage and a detailed view of Chaos Injection in Thundra, you can read our [blog post](https://blog.thundra.io/chaos-test-your-lambda-functions-with-thundra)!

To learn more about configuring chaos engineering in each environment, explore our documentation pages: [Java](https://apm.docs.thundra.io/java/configuration-options/chaos-injection), [Python](https://apm.docs.thundra.io/python/configuration-options/chaos-injection#pyth), [NodeJS](https://apm.docs.thundra.io/node.js/nodejs-configuration-options/chaos-injection).

## Wrapping Up <a href="#sub_4" id="sub_4"></a>

This guide is intended to be a pathfinder for not only Epsagon customers but anyone who is adopting/had already adopted serverless technology to their stack.

Please don't forget to look at Thundra APM [documentation](https://apm.docs.thundra.io/) and [sign up](https://start.thundra.io/apps) for it.
