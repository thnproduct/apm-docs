# Serverless Metrics Tab

The Metric tab gives an overall picture of all the invoked functions that fall within a specified filter. The invocation data used to render the metric graphs are from those invocations that fall under the time interval selected using the Time Setting of the Function Details page.

The graphs shown vary according to the runtime of the Lambda function for which the invocation is being analyzed. This is because each runtime has varying metrics concerning specific runtime, and Thundra ensures that detailed and relevant monitoring data is presented.

### Graphs Coming from AWS Integration

When you connect Thundra to your AWS account, we have access to some metrics that are also available with Amazon CloudWatch metrics. These metrics include:

#### Invocation Count

Shows the invocation, error, and throttle count with a trendline.

#### Duration

Shows the average, p50, p90, and p99 of the whole invocations.

#### Provisioned Concurrency

AWS just announced that you can now provision a definite number of containers for your system. Thundra shows how you are using your provisioned concurrent invocations and spillover invocations. As you can see in the example below, provisioned concurrency invocations increase up to 5 (which is the provision concurrency limit). Once this limit is reached, the spill-over invocation count increases because all provisioned concurrency is occupied already.

![](https://files.readme.io/0818ad3-image\_44.png)

### Graphs Per Runtime

#### Node.js

**Invocation counts:** Shows total cold starts and faulty (ended with thrown exception) invocation counts for the selected function for each time interval.

**Invocation durations:** Shows the average duration of total cold starts and faulty (ended with thrown exception) invocations in milliseconds for the selected function for each time interval.

**Process memory usages: S**hows the average process memory usages in megabytes (MB).

**Memory usages:** Shows the average container’s memory usages in megabytes (MB).

**CPU percentages:** Shows the average process’s CPU usage percentages.

**Disk IO:** Shows the average Disk IO in bytes.

**Thread Count:** Shows the number of active thread counts.

#### Python

**Invocation counts:** Shows total cold starts and faulty (ended with thrown exception) invocation counts for the selected function for each time interval.

**Invocation durations:** Shows the average duration of total cold starts and faulty (ended with thrown exception) invocations in milliseconds for the selected function for each time interval.

**CPU metrics:** Shows the average process’s and system’s CPU usage percentages.

**GC counts:** Shows how many GC is executed for generation 0, generation 1, and generation 2.

**Process memory metrics:** Shows the process’s average memory usages in megabytes (MB).

**System memory metrics:** Shows the system’s average memory usages in megabytes (MB).

**Thread counts:** Shows the number of active thread counts.

#### Go

**Invocation counts:** Shows total cold starts and faulty (ended with thrown exception) invocation counts for the selected function for each time interval.

**Invocation durations:** Shows the average duration of total cold starts and faulty (ended with thrown exception) invocations in milliseconds for the selected function for each time interval.

**Number of Go-routines:** Shows average number of Go-routines in execution.

**GC CPU percentage:** Shows average of what percentage of CPU is used by Garbage Collector.

**GC pause:** Shows the number of GC stop-the-world pauses since the program started in milliseconds (ms).

**GC counts:** Shows average of how many GCs are executed.

**Heap stats (MB):** Heap stats on average.

**Heap Allocation:** MBs of allocated heap objects. "Allocated" heap objects include all reachable objects, as well as unreachable objects that the garbage collector has not yet freed.

**Heap In Use:** MBs in in-use spans, which have at least one object in them. These spans can only be used for other objects that are roughly the same size.

**The number of allocated heap objects:** Shows the average number of allocated heap objects.

**CPU percentage:** Shows the average percentage of the CPU used by this Lambda invocation.

**IO-Stats (KB):** Shows average disk read and disk write in KBs.

**Network IO-Stats (KB):** Shows average network IO operations based on how many bytes are sent and received.

**Network IO error counts:** Shows the number of errors made while sending and receiving packets in total.

#### Java

**Invocation counts:** Shows total cold starts and faulty (ended with thrown exception) invocation counts for the selected function for each time interval.

**Invocation durations:** Shows the average duration of total cold starts and faulty (ended with thrown exception) invocations in milliseconds for the selected function for each time interval.

**Thread counts:** Shows the average of active thread counts and maximum of peak thread counts for the selected function for each time interval.

**Loaded class counts:** Shows the average of currently loaded class counts and the maximum of total loaded class counts for the selected function for each time interval.

**Memory usages:** Shows the average JVM heap and non-heap memory usages in MB for the selected function for each time interval.

**Memory usages by pools:** Show average memory usages of each JVM memory region in MB for the selected function for each time interval. The following memory regions are shown:

* Eden space
* Survivor space
* Tenured generation
* Metaspace
* Code cache

**CPU percentages:** Shows the average process CPU usage percentage of the selected function for each time interval.&#x20;

**GC CPU percentages:** Shows the average minor and major GC originated CPU usage percentages of the selected function for each time interval.

**GC counts:** Shows the total minor and major GC counts of the selected function for each time interval.

**GC durations:** Shows the total minor and major GC durations in milliseconds for the selected function for each time interval.

#### .NET

**Invocation counts:** Shows total cold starts and faulty (ended with thrown exception) invocation counts for the selected function for each time interval.

**Invocation durations:** Shows the average duration of total cold starts and faulty (ended with thrown exception) invocations in milliseconds for the selected function for each time interval.

**CPU percentages:** Shows the average process CPU usage percentage of the selected function for each time interval.

**Memory usages:** Shows the average memory usage of the allocated memory in MB for the selected function for each time interval.
