# Chaos Engineering with Thundra

While developing an application, bottlenecks and failures in your business logic or infrastructure are inevitable, and it’s important to find and fix these issues before your application goes to production. Testing your system before it causes more damage in production is imperative to avoid affecting users. Using controlled experiments to identify the behaviour of your system when a fallback happens is called [**Chaos Engineering**](http://principlesofchaos.org/?lang=ENcontent)**.** That’s why Thundra has incorporated this process into its services with the Chaos Injection feature.

This feature allows you to inject errors to your Lambda functions to simulate failures in your serverless system. Thundra currently supports Chaos Injection in Java, Node.js, and Python.

### What is a Span?

A span is a concept that Thundra inherited from Opentracing, and is basically an individual unit of work done in your Lambda functions. You can create custom spans using Thundra, but if you use automated instrumentation for your functions, Thundra will also create spans for you. In the Thundra console, you can display these spans under an invocation's trace chart. Thundra supports the following integrations for spans:

* Botocore
* Dynamodb
* SQS
* SNS
* Kinesis
* Firehose
* S3
* Lambda
* MySQL
* PostgreSQL
* Redis
* Requests (HTTP)

Even if you don’t create a span manually, Thundra automatically creates a span whenever you make a Redis call, HTTP call, DynamoDB call, etc., and adds necessary tags related to that specific operation.

### What Are Span Listeners?

Span listeners are classes that inherit attributes from `ThundraSpanListener` in order to create spans in your Lambda function using methods such as `on_span_started(span)` and `on_span_finished(span)`. You can create a span using span listeners and perform custom operations in the life cycle of this span. A span listener can be used to perform fallbacks in your functions. Thundra provides you with three ready-to-use span listeners for Chaos Injection:

* ``[`ErrorInjectorSpanListener`](chaos-engineering-with-thundra.md#errorinjectorspanlistener)``
* ``[`LatencyInjectorSpanListener`](chaos-engineering-with-thundra.md#latencyinjectorspanlistener)``
* ``[`FilteringSpanListener`](chaos-engineering-with-thundra.md#filteringspanlistener)``

You can manually add these listeners to your code to see the behaviour of your serverless system when a problem occurs.

### How to Inject Chaos?

For an example usage and a detailed view of Chaos Injection in Thundra, you can read our [blog post](https://blog.thundra.io/chaos-test-your-lambda-functions-with-thundra)!

To learn more about configuring chaos engineering in each environment, explore our documentation pages: [Java](../java/configuration-options/chaos-injection.md), [Python](../python/configuration-options/chaos-injection.md#pyth), [NodeJS](../node.js/nodejs-configuration-options/chaos-injection.md).
