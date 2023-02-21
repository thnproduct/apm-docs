# Digging Observability Data with Thundra Queries

With a combination of automated instrumentation and the capability to enact manual instrumentation, Thundra hosts rich data that enables you to better understand your applications. Thundra's highly configurable and extensible query language allows you to ask any question and find the answer in our data.

Using queries, you can:

* Dig through enriched function, invocation, log, trace, and operation data and ask meaningful questions.
* Save queries for later use or share them with colleagues, making it possible for everyone to have a common view of the application.
* Set up detailed alerts and assign a severity level to them.

Note that pre-automated queries can save you time, but you can also build your own queries using Thundra's Query Helper with no need to know the syntax.

### Applications&#x20;

Listed below are different queries that you can use to explore your functions from different aspects.

{% hint style="info" %}
You can use the Query Helper for applications to build visual queries. For more information, see the [Query Helper for Functions](https://docs.thundra.io/thundra-web-console/query-helper/query-helper-for-functions).
{% endhint %}

To list applications in a specific region:

```
Region=eu-west-1 ORDER BY LastInvocationTime 
```

To list applications with more than a specific count of timeouts:

```
COUNT(Timeout) >=10 ORDER BY LastInvocationTime DESC
```

To list applications with custom tags that you set for your functions _(serviceName is used as a custom tag here):_

```
appTags.serviceName IN (user,team) 
ORDER BY LastInvocationTime DESC
```

{% hint style="info" %}
To learn how to use tags for applications, see this [blog](https://blog.thundra.io/enhancing-distributed-tracing-with-business-context).&#x20;
{% endhint %}

To combine two (or more) conditions together, you can use the AND keyword. For example, the following query will return applications that have the string tag of "user" and with healthy (normal-erroneous/normal \* 100) invocations that are less than 95 percent of the total:

```
appTags.serviceName=user AND 
Health < 95 
ORDER BY LastInvocationTime DESC
```

You can sort applications according to various fields, such as health, last invocation time, number of errors or invocations, and even cost. In the following example, the previous query will run but applications will be sorted according to cost in the selected time period.

```
appTags.serviceName=user AND 
Health < 95 
ORDER BY EstimatedCost DESC
```

### Invocations&#x20;

Listed below are different queries that you can use to explore the invocations of an application from different aspects.&#x20;

{% hint style="info" %}
You can use the Query Helper for invocations to build visual queries. For more information, see the[ Query Helper for Invocations](https://docs.thundra.io/thundra-web-console/query-helper/query-helper-for-invocations).
{% endhint %}

To list the latest erroneous invocations:

```
Erroneous=true ORDER BY LastInvocationTime DESC 
```

To list invocations with a specific error type:

```
ErrorType=DemoIllegalAccessException 
ORDER BY LastInvocationTime DESC
```

To list invocations with business tags that you set for your invocations _(user.id is used as a custom tag here):_

```
tags.user.id="1" 
ORDER BY LastInvocationTime DESC
```

{% hint style="info" %}
To learn how to use tags for invocations, see this [blog](https://blog.thundra.io/enhancing-distributed-tracing-with-business-context).&#x20;
{% endhint %}

To combine two (or more) conditions together, you can use the AND keyword. For example, the following query will return invocations that have the string tag of "user.id" and that have a duration lasting longer than a second (1000 ms).

```
tags.user.id="1" AND 
Duration > 1000 
ORDER BY LastInvocationTime DESC
```

You can sort invocations according to various fields, such as duration and last invocation time. In the following example, the previous query will run but invocations will be sorted according to their duration, from longest to shortest.

```
tags.user.id="1" AND 
Duration > 1000 
ORDER BY Duration DESC
```

### Unique Traces&#x20;

Unique traces represent the unique business flows that occur multiple times in a distributed architecture. For example, the same five Lambda functions can be triggered asynchronously when an e-commerce user adds an item to cart. When this happens, the asynchronous chain of invocations is defined as a "unique trace" in Thundra.&#x20;

Listed below are different queries that you can use to explore unique traces from different aspects.&#x20;

{% hint style="info" %}
You can use the Query Helper for unique traces to build visual queries. For more information, see the[ ](https://docs.thundra.io/thundra-web-console/query-helper/query-helper-for-invocations)[Query Helper for Unique Traces.](../thundra-web-console/query-helper/query-helper-for-unique-traces.md)
{% endhint %}

To list unique traces that contain an invocation of a specific application:

```
Name=user-team-api-java-lab ORDER BY COUNT(Error) DESC 
```

To list unique traces that have more errors than a set threshold (in the example below, 20):

```
COUNT(Error) >20 ORDER BY COUNT(Error) DESC
```

To list unique traces with at least one SQS interaction that has a duration lasting longer than 100ms:

```
resource.AWS-SQS.duration>100 
ORDER BY COUNT(Trace) DESC
```

To combine two (or more) conditions together, you can use the AND keyword. For example, the following query will return unique traces that lasted longer than two seconds and that have at least one SQS interaction with a duration lasting longer than 100ms:

```
resource.AWS-SQS.duration=100 
AND AVG(Duration) >2000 
ORDER BY COUNT(Error) DESC
```

### Traces

A trace is one occurrence of a business flow in the system. Listed below are different queries that you can use to explore traces from different aspects.&#x20;

{% hint style="info" %}
You can use the Query Helper for traces to build visual queries. For more information, see the [Query Helper for Traces](../thundra-web-console/query-helper/query-helper-for-traces.md) page.
{% endhint %}

To list traces that have at least one cold-start invocation:

```
HasColdStart=true ORDER BY StartTime DESC
```

To list traces that have a duration lasting longer than a second (1000ms):

```
Duration > 1000 ORDER BY StartTime DESC
```

To combine two (or more) conditions together, you can use the AND keyword. For example, the following query will return traces that have at least one erroneous invocation and that have a duration lasting longer than 10 seconds (10000ms):

```
HasError=true 
AND Duration > 1000 
ORDER BY StartTime DESC
```

### Operations

An operation is a single interaction between a compute resource and any other resource. These can be referenced in the application from [here](https://console.thundra.io/operations/default/1). Listed below are different queries that you can use to explore operations from different aspects.

{% hint style="info" %}
You can use the Query Helper for operations to build visual queries. For more information, see the[ ](https://docs.thundra.io/thundra-web-console/query-helper/query-helper-for-invocations)[Query Helper for  Operations](https://docs.thundra.io/thundra-web-console/query-helper/query-helper-for-operations).
{% endhint %}

To list erroneous operations:

```
Erroneous=true ORDER BY StartTime DESC
```

To list operations that took more than a second:

```
 Duration > 100 ORDER BY StartTime DESC
```

To combine two (or more conditions) together, you can use the AND keyword. For example, the following query will return the DynamoDB operations that have a duration lasting longer than 1000ms:

```
ResourceType=AWS-DynamoDB 
AND 
Duration > 1000 ORDER BY StartTime DESC
```

### Logs

Logs are essentially every single log that your applications print. You can check [this page](https://console.thundra.io/logs/) or more information if needed.

{% hint style="info" %}
You can use the Query Helper for logs to build visual queries. For more information, see the[ Query Helper for Logs](../thundra-web-console/query-helper/query-helper-for-logs.md) page.
{% endhint %}

To list log items with a log level of "INFO":

```
Level=INFO ORDER BY Time DESC
```

To list log items that have the string value that's similar to the “err” string:

```
Message=*err* ORDER BY Time DESC
```

To combine two (or more) conditions together, you can use the AND keyword. For example, the following query will return logs that have the "err" string and a log level of "INFO":

```
Message=*err* AND Level=INFO ORDER BY Time DESC
```

