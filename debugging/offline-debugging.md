# Time-Travel Debugging

Due to the black-box nature of serverless platforms, debugging serverless applications is a cumbersome task as application teams lose access to the underlying server. Developers therefore have to be content with what they have available: logs. However, logs tend to be short on detail in order to demonstrate the behavior of AWS Lambda applications because they can only reflect the print statements in the code. When you want to achieve a granular level of understanding using logs, you need to undergo manual work that's not only time-intensive, but costly in terms of additional log ingestion and storage prices.

Thundra makes this task easier with its unmatched offline debugging experience that provides an IDE-like experience for invocations reported to Thundra.

In the figure below, Thundra displays the code in a Lambda function as it was presented on the developer’s IDE. Using offline debugging, developers can navigate between lines of code and track changes in the local variables and function parameters. Plus, developers can jump into children functions or AWS SDK, HTTP, Redis, and MySQL calls as they step into the calls in their IDE.

![](<../.gitbook/assets/image (171).png>)

This process is called offline debugging, or “non-breaking debugging,” where developers walk through a function invocation after it's completed. This ability to replay what happened in the invocation means we can transform serverless compute into white-box.

Offline debugging is available for Java, Python, and Node.js runtimes. In order to use Thundra’s offline debugging feature, you'll need to update your Thundra libraries to the newer versions listed below:

* Node.js: 2.8.0, layer 36 or higher
* Python: 2.4.4, layer 19 or higher.
* Java: 2.4.9, layer 39 or higher.

You can enable offline debugging by configuring the environment variables of Lambda functions. Please see our detailed explanations for [Node.js](https://docs.thundra.io/node.js/enrich-tracing#offline-debugging), [Python](https://docs.thundra.io/python/enrich-tracing#offline-debugging) and [Java](https://docs.thundra.io/java/enrich-tracing#offline-debugging).&#x20;
