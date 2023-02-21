# Instrument to Achieve Deeper Level of Visibility

If you're developing a distributed system composed of serverless functions, containers, and VMs, it is important to be able to trace your transactions end-to-end with great detail. You need to keep your system stable, be aware of bottlenecks in your application, and measure your system performance to keep it at an optimum level. In serverless-centric environments, it's harder to understand what actually happened in and between functions due to the distributed nature of serverless. Logs can help to some extent, but they are not as helpful as they should be when it comes to revealing why your transactions are slow. In order to understand what's going on, you'll need to instrument your function and see the traces representing the life cycle of distributed transactions in a timeline. Thundra is the only vendor that can provide end-to-end visibility through instrumentation. “End-to-end” means understanding and managing the aggregate set of distributed services an application consumes, right down to the line level of the runtime code for every local service or container-based application.

Connecting Thundra to your AWS account helps you display basic data, such as logs and metrics of your functions and invocations. For some people, this might be enough, but if you're looking for a deeper level of visibility and architectural overview in your system, you will need to step into instrumentation.

To instrument your code, you have three options:

* Manual instrumentation
* Automated instrumentation
* Instrumentation through the Thundra console

### Manual Instrumentation

With manual instrumentation, you can add custom spans to your function. These spans will be displayed on Thundra, and they allow you to observe your function behavior. For example, you can start a span while your code begins to run a complex algorithm, and then close the span at the end of the runtime. You can set custom tags in your span in order to see what actually happened in this code block.

To manually instrument the Lambda function, you need to make code changes. You can get more detailed information on how to instrument your function for each environment: [Java](../../node.js/enrich-tracing.md), [Python](../../python/enrich-tracing.md#manual-instrumentation-with-open-tracing), [NodeJS](../../node.js/enrich-tracing.md), [.NET](../../.net/enrich-tracing.md#manual-instrumentation-with-opentracing) and [Go](../../go/enrich-tracing.md).

### Automated Instrumentation

Automated instrumentation also allows you to measure the execution time for the Lambda code. By instrumenting your Lambda function, you are able to have end-to-end visibility with no code change required. By default, Thundra makes automated instrumentation for AWS resources (except for Kinesis and Firehose), HTTP endpoints, Redis caches, MySQL, PostgreSQL tables, and so on.

You can find more detailed information on how to instrument your Lambda function automatically for each environment: [Java](../../java/configuration-options/trace-configuration.md), [Python](../../python/configuration-options/trace-configurations.md), [NodeJS](../../node.js/nodejs-configuration-options/trace-configurations.md), .[NET](../../.net/configuration-options/trace-configurations.md) and [Go](../../go/go-configuration-options/trace-configurations.md).

### Instrumentation through Console

Even if it's very straightforward to install Thundra libraries in each runtime with Lambda Layers, AWS SAM, or any other tool, this process still requires a modification in .yaml files and/or function configuration in Lambda functions.

In order to alleviate this manual process, you can instrument your functions directly from the console. To do this, you have to connect your AWS account to Thundra from[ here](https://console.thundra.io/settings/aws). Note that Thundra updates your function configuration in your AWS account with this option. The steps that are automatically conducted are:

* Adding Thundra Layer to your function.
* Changing the runtime of your function to a custom runtime provided by Thundra (for Node.js, .NET, and Java).
* Assigning a Thundra API key.

You can get more information about how to do this [here](../../thundra-web-console/functions-list-page/#instrumentation).&#x20;

