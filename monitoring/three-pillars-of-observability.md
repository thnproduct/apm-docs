# Three Pillars of Observability

The power of observability for applications cannot be underestimated. However, observability should not be limited to collecting only one type of data, as this only provides one facet of information. If you want deeper insight about a serverless system, you need to collect data from multiple sources and then combine them in a meaningful way to achieve seamlessly operating serverless applications. Thundra offers three pillars of observability to provide insight about the behavior of a system: Metrics, Logs, and Traces.

### Metrics

In Thundra, metrics quickly and easily show you what to look out for, and are provided in a visual display so you can quickly and easily understand how your serverless system is running. Different types of [metrics](../thundra-web-console/function-details-page/metrics-tab.md) are available for your Lambda functions, allowing you to observe their behavior within time intervals. When you plug Thundra into a Lambda function, the following metrics can be differentiated according to the function’s runtime:

* **Invocation Counts -** Shows the total number of invocations that ended with an error or had a cold start.
* **Invocation Durations** - Shows the average, p99, and p95 duration of invocations for each time interval.
* **Memory Usages** - Shows the average memory usage of the allocated memory within each time interval.
* **CPU Percentages** - Shows the average process CPU usage of a selected function for each time interval.
* **Disk IO Bytes** - Shows the average disk IO bytes for a selected function within each time interval.
* **Process Memory Usages** - Shows the average process memory usage within each time interval for a selected function.
* **Thread Count** - Shows the activated thread count for each time interval.
* **GC Counts** - Shows how many GC were executed for generation 0, generation 1, and generation 2.
* **Number of Go-Routines (Go)** - Shows the number of Go-routines in execution on average.
* **GC Pause (Go)** - Shows the GC stop-the-world pauses in milliseconds (ms) since the program started.
* **Heap Stats (Go)** - Shows heap stats on average, including:
  * Heap Allocation: MBs of allocated heap objects. "Allocated" heap objects include all reachable and unreachable objects that the garbage collector has not yet freed.
  * Heap In-Use: MBs of in-use spans. In-use spans have at least one object and can only be used for other objects that are roughly the same size.
* **Number of Allocated Heap Objects (Go)** - Shows the average number of allocated heap objects within each time interval.
* **Network IO Stats (Go)** - Shows the average network IO operations of how many KBs are sent or received.
* **Network IO Error Counts (Go)** - Shows the number of errors in total while sending and receiving packets.
* **Loaded Class Counts (Java)** - Shows the average of currently loaded class counts and the maximum of total loaded class counts of a selected function for each time interval.
* **Memory Usages by Pools (Java)** - Shows the average memory usages of each JVM memory region in MBs in a selected function for each time interval. The following memory regions are shown:
  * Eden Space
  * Survivor Space
  * Tenured Generation
  * Metaspace
  * Code Cache
* **GC Durations (Java)** - Shows the total minor and major GC durations in milliseconds of a selected function for each time interval.

You can navigate to the metrics of your function by clicking on the Functions List page and then selecting the Metrics Tab on the Function Details page.

To display your function metrics on Thundra, learn how to configure them  on the following pages:&#x20;

* [NodeJS](../node.js/nodejs-configuration-options/metric-configurations.md)
* [Python](../python/configuration-options/metric-configurations.md)
* [Java](../java/configuration-options/metrics-configurations.md)
* [Go](../go/go-configuration-options/metric-configurations.md)
* [.NET](../.net/configuration-options/metric-configurations.md)

### Traces

Traces allow you to observe your transactions end-to-end with a concise look into your functions, showing you what happens to them and when. This gives you more control over your functions and helps you easily pinpoint performance issues and bottlenecks.&#x20;

#### Open Tracing Compatibility

Thundra agents support the OpenTracing API. If you are not familiar with OpenTracing, you can get detailed information [here](https://opentracing.io/). When you plug Thundra agents into your Lambda function, it automatically creates a root span for your function. You can access Thundra’s tracer via OpenTracing’s GlobalTracer instance, and then build new spans and add business information to increase observability.

You can also add individual spans to parts of your function in order to better monitor it.

![Example Trace Map](<../.gitbook/assets/image (152).png>)

#### Distributed Traces

Thundra provides a "Full Tracing" capability, which means you have both distributed and local tracing available to use. With distributed tracing, you can observe the interactions of your Lambda functions with other resources, whether from a higher level or down to the lines of code.

Distributed tracing is supported with two unique capabilities in Thundra:

* Multiple upstream transaction - There can be multiple invocations when an invocation is triggered. For example, a batch of messages can come through multiple invocations and yet still be written to DynamoDB with one transaction. In Thundra, you can display each upstream invocation link to the downstream invocation.
* Business transaction - Some invocations are related to each other with logical interactions, such as writing a message to DynamoDB table after an approval. You can link logically related flows with Thundra.

#### Local Tracing

![](<../.gitbook/assets/image (183).png>)

Local tracing enables you to trace methods in your Lambda function. On the [Trace Chart](../thundra-web-console/invocation-detail-page/trace-chart-tab.md) page, spans of an invocation displayed. Methods in your Lambda function can be seen with the Method tag; just click on a method to display its details of it. By clicking on the Summary tab, you can also display local variables and parameters inside a method within a Lambda function.

#### Trace Search

The Traces page lists all the traces on your serverless system, which can be filtered and searched using Thundra’s detailed query capability. You can display a map of a specific trace by clicking on any trace in the list.

![](<../.gitbook/assets/image (193).png>)

Using this trace map, you can see the health of interactions between your Lambda function and other resources. By clicking on your Lambda function, you can find detailed insight about it, including displaying line by line of code, line logs, local variables, and tags.

For more detailed information about how to configure your serverless system to achieve tracing capabilities, visit the following pages:&#x20;

* [NodeJS](../node.js/nodejs-configuration-options/trace-configurations.md)
* [Python](../python/configuration-options/trace-configurations.md)
* [Java](../java/configuration-options/trace-configuration.md)
* [Go](../go/go-configuration-options/trace-configurations.md)
* .[NET](../.net/configuration-options/trace-configurations.md).&#x20;

### Logs

Logs are the most common resource that developers use to collect data and quickly debug their code, as logs provide more detailed data than metrics do.  Thundra allows you to display your logs and see the connection between your invocations and traces with just one click of your mouse!

![](<../.gitbook/assets/image (254).png>)

You can display a list of your logs on the Logs page, and investigate them further using Thundra's detailed query capability. In addition,  traces and metrics are also aggregated. On this page, you can display metrics of a function and display logs of any invocation of this function, and for each span in a trace, logs can be displayed.

You can add logs to your function using console.log() or the Thundra logger. For more detailed information, see the following pages:&#x20;

* [Java](../java/configuration-options/log-configurations.md)
* [Python](../python/configuration-options/log-configurations.md)
* [.NET](../.net/configuration-options/log-configurations.md)
* [Go](../go/go-configuration-options/log-configurations.md)
* [Node.js](../node.js/nodejs-configuration-options/log-configurations.md)
