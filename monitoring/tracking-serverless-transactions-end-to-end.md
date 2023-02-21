# Tracking Serverless Transactions End-to-End

In today's modern software development, distributed applications have become more popular – and by adopting distributed serverless systems, we can expect the process of building applications will be faster and highly scalable. Despite these benefits, distributed serverless systems do have challenges when it comes to monitoring and troubleshooting. Users working with distributed applications need to display their serverless system with visibility into services and their interactions.&#x20;

Thundra offers users the capability to track their serverless distributed system from end-to-end with **Full Tracing**, which provides both distributed tracing and local tracing. Full Tracing allows you to monitor your Lambda functions from a high level, such as interactions with other Lambdas and services, to a deeper level, right down to lines of code.

Distributed tracing is supported with two unique capabilities in Thundra:

* Multiple upstream transactions - There can be multiple invocations when just one invocation is triggered. For example, a batch of messages can come through multiple invocations and be written to DynamoDB with one transaction. In Thundra, you can display each upstream invocation link to a downstream invocation.
* Business transaction - Some invocations are related to each other with logical interactions, such as writing a message to a DynamoDB table after approval. You can link logically related flows with Thundra's distributed tracing.

Thundra provides this capability via Traces. You can display your all individual traces on the  [Traces Page](../thundra-web-console/traces-page.md). Using detailed queries, you can filter your traces in terms of:

* Trigger of the origin
* Entry point
* Start time
* End-to-end duration of the trace
* Interacted resources
* Error types, if any are thrown
* Duration breakdown

When you want to go through a review on a deeper level of your serverless system, filter your traces according to your needs and click on any trace. The [Trace map page](../thundra-web-console/traces-page.md#trace-details) will be shown as below:

![Trace Map](<../.gitbook/assets/image (243).png>)

The Trace Map provides you with a view of interactions between Lambda functions in a specific trace, along with metrics and the health of interactions. As seen in the image below, a failed Lambda function is highlighted with a red square. You can click on the failed Lambda function to figure out what's going wrong.

![Trace Details](<../.gitbook/assets/image (300).png>)

Side view is an easy way to detect the root cause of issues in a problematic node in a specific trace. With one click, you can investigate your CloudWatch logs or details of the invocation of that trace.

### How to configure your serverless system for distributed tracing

To use end-to-end tracing, you can easily configure your system by going through our documentation on the following pages:

* [Java](../java/configuration-options/trace-configuration.md)
* &#x20;[Go](../go/go-configuration-options/trace-configurations.md)
* .[NET](../.net/configuration-options/trace-configurations.md)&#x20;
* [NodeJS](../node.js/nodejs-configuration-options/trace-configurations.md)
* [Python](../python/configuration-options/trace-configurations.md)

