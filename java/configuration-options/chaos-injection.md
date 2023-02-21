# Chaos Injection in Java SDK

## Span Listeners

By their nature, serverless systems are distributed systems. When using serverless architectures, which are highly distributed and event-driven, you should consider being robust in case latencies and errors happen with any component of your system. For example, you should be prepared if you get a connection error while trying to reach your Redis cache, and should any of the components respond with a latency that slows down the flow, you should have a plan to ensure your system doesn’t collapse. Likewise, you should think ahead – for instance, if you are calling a Lambda inside another Lambda, make sure first that the outer one has bigger timeouts.

It can seem trivial to test applications in this sense, but things will become more complicated as your architecture grows. We developed our `SpanListener` and `SpanFilterer` APIs for this purpose: to test your system and find weak points. When you want to inject chaos to your distributed system, you can use Thundra’s `SpanListener` API to create latency or an error that your system may face by only playing with the environment variables, and not changing your code.&#x20;

### What is a span?

A span is essentially an individual unit of work done in your Lambda functions, a concept that Thundra inherited from Opentracing. You can manually create spans using Thundra; however, thanks to the integrations that Thundra has, spans are automatically created for you if you use the Traceable annotation or our integrations. Currently, Thundra has the following integrations:

* AWS services (Lambda, DynamoDB, SQS, SNS, Kinesis, Firehose, S3, API Gateway, ...)
* MySQL
* PostgreSQL
* Microsoft SQL Server
* MariaDB
* Redis
* Elasticsearch
* HTTP

These integrations mean that even if you don’t create a span manually, whenever you make a Redis call, HTTP call, DynamoDB call, etc., Thundra automatically creates a span for that call and adds the necessary tags related to that specific operation.

### What is a Span Listener?

There are three out-of-the-box `SpanListener` implementations in Thundra’s Java Agent:

* `ErrorInjectorSpanListener`
* `LatencyInjectorSpanListener`
* `FilteringSpanListener`

### `ErrorInjectorSpanListener`

Parameters include:

* **errorMessage**: The message that displays the error description when using the `ErrorInjectorSpanListener`. The default value is “Error injected by Thundra!”.
* **errorType**: The type of error that will be raised. The name of the thrown error will be set to this value. The default value is `java.lang.RuntimeException`.
* **injectOnFinish**: The Boolean value that decides whether the error will be injected after the span is finished or before it starts. The default value is `false`.
* **injectPercentage**: The integer value that decides the percentage for error injection. For example, if this value is `10` (i.e., 10%), every 10th span event will be an error. The default value is `100`.

{% code title="ErrorInjectorSpanListener" %}
```java
import io.thundra.agent.trace.TraceSupport;
import io.thundra.agent.trace.span.impl.ErrorInjectorSpanListener;

...
  
// Create span listener  
ErrorInjectorSpanListener spanListener = 
    new ErrorInjectorSpanListener(
  	        false, // don't inject on finish but on start before the actual operation
            java.lang.IllegalAccessException.class, // type of the exception to be thrown
            "No way!", // the error message
  	        50 // inject error at 50% percentage 
	);

// Register span listener
TraceSupport.registerSpanListener(spanListener);
```
{% endcode %}

### `LatencyInjectorSpanListener`

Parameters include:

* **delay**: The integer value that decides the amount of latency that will be injected to the spans in milliseconds. The default value is `100` ms.
* **randomizeDelay**: The Boolean value that enables randomized amounts of injected delays. For example, if the delay is `100` ms and randomization is enabled, a random number is generated between `1` and `100` as the amount of each injected latency in milliseconds. The default value is `false`.
* **injectOnFinish**: The Boolean value that decides whether the latency will be injected after the span is finished or before it starts. The default value is `false`.

{% code title="LatencyInjectorSpanListener" %}
```java
import io.thundra.agent.trace.TraceSupport;
import io.thundra.agent.trace.span.impl.LatencyInjectorSpanListener;

...
  
// Create span listener  
LatencyInjectorSpanListener spanListener = 
    new LatencyInjectorSpanListener(
  	        false, // don't inject on finish but on start before the actual operation
            50, // amount of delay to be injected in milliseconds
            true // randomize amount of injected delays between 1 and 50 (configured delay)
	);

// Register span listener
TraceSupport.registerSpanListener(spanListener);
```
{% endcode %}

### `FilteringSpanListener`

Parameters include:

* **listener**: If the span is accepted by the `filterer`, the `listener` is then notified. There is no default value as this parameter is mandatory.
* **filterer**: The `filterer` is the main object that decides whether or not the given span will be delegated to the `listener` parameter, meaning it is basically an object that has an accept method. Before delegating the given span to the `listener` parameter, `FilteringSpanListener` first passes the given span to the `filterer` object if the filterer’s accept method returns `true`. If it does, then the given span is passed to the `listener` parameter; if it does not, the given span is not passed to the `listener`. There is no default value as this parameter is mandatory.

For example:

{% code title="FilteringSpanListener" %}
```java
import io.thundra.agent.trace.TraceSupport;
import io.thundra.agent.trace.span.impl.ErrorInjectorSpanListener;
import io.thundra.agent.trace.span.impl.FilteringSpanListener;
import io.thundra.agent.trace.instrument.integrations.redis.RedisTags;

...
  
// Create error injector span listener  
ErrorInjectorSpanListener listener = 
    new ErrorInjectorSpanListener(
  	        false, // don't inject on finish but on start before the actual operation
            java.lang.IllegalAccessException.class, // type of the exception to be thrown
            "No way!", // the error message
  	        50 // inject error at 50% percentage 
	);

// Create filterer to decide which spans will be listened  
FilteringSpanListener.SpanFilterer filterer =
	new FilteringSpanListener.SpanFilterer() {
		@Override
        public boolean accept(ThundraSpan span) {
    	    return "Redis".equals(span.getClassName()) // Targeting only Redis operations
        		   &&
                   "GET".equalsIgnoreCase(span.getTag(RedisTags.COMMAND)); // Targeting only Redis "GET" commands
        }
	};

// Create filtering span listener  
FilteringSpanListener filteringSpanListener =
    new FilteringSpanListener(
		    listener,
            filterer
	);

// Register span listener
TraceSupport.registerSpanListener(filteringSpanListener);
```
{% endcode %}

The same configuration can be done over `SpanFilter`:

{% code title="FilteringSpanListener" %}
```java
import io.thundra.agent.trace.TraceSupport;
import io.thundra.agent.trace.span.impl.ErrorInjectorSpanListener;
import io.thundra.agent.trace.span.impl.FilteringSpanListener;
import io.thundra.agent.trace.instrument.integrations.redis.RedisTags;

import java.util.Arrays;
import java.util.HashMap;

...
  
// Create error injector span listener  
ErrorInjectorSpanListener listener = 
    new ErrorInjectorSpanListener(
  	        false, // don't inject on finish but on start before the actual operation
            java.lang.IllegalAccessException.class, // type of the exception to be thrown
            "No way!", // the error message
  	        50 // inject error at 50% percentage 
	);

// Create filterer to decide which spans will be listened  
FilteringSpanListener.SpanFilter filter =
	new FilteringSpanListener.SpanFilter(
  	        null, // "domainName" is not checked
            "Redis", // Targeting only Redis operations
            null, // "operationName" is not checked
            new HashMap() {{
    	        put(RedisTags.COMMAND, "GET"); // Targeting only Redis "GET" commands
            }}
	);

// Create filtering span listener  
FilteringSpanListener filteringSpanListener =
    new FilteringSpanListener(
		    listener,
            Arrays.asList(filter)
	);

// Register span listener
TraceSupport.registerSpanListener(filteringSpanListener);
```
{% endcode %}

**Configuration using environment variables:**\
In order to create a span listener using an environment variable, you should set `thundra_agent_lambda_trace_span_listenerConfig` as a prefixed (multiple listener definitions can have a different suffix) environment variable in JSON format. The basic structure is similar to the following:

{% code title="Basic structure of a span listener created using the environment variable" %}
```yaml
thundra_agent_lambda_trace_span_listenerConfig...: '{"type":"<span-listener-name>","config":{[<arguments>]}'
```
{% endcode %}

* `<span-listener-name>` can be the full classname of one of the following predefined `SpanListeners` (`ErrorInjectorSpanListener`, `LatencyInjectorSpanListener`, or `FilteringSpanListener`).
* `<arguments>` are lists of objects that have key-value pairs.

Here is an example configuration that creates the same filtering span listener that we created programmatically above:

{% code title="A FilteringSpanListener configured using the environment variable" %}
```yaml
thundra_agent_lambda_trace_span_listenerConfig:'{"type":"FilteringSpanListener","config":{"listener":{"type":"ErrorInjectorSpanListener","config":{"errorType":"java.lang.IllegalAccessException","errorMessage":"Noway!","injectPercentage":50}},"filters":[{"className":"Redis","tags":{"redis.command":"GET"}}]}}'
```
{% endcode %}

There are a couple of points to note here:

* Using the "`type`":"`LatencyInjectorSpanListener`", we state the type of the listener that `FilteringSpanListener` is going to use. You can also use LatencyInjectorSpanListener when you need to inject latency.
* Key-value pairs under the config property are passed to the wrapped span listener, which is specified by property type (which in this case is `ErrorInjectorSpanListener`) as parameters.
* Key-value pairs under the config property are used to define individual filters. For example,`{"className":"Redis","tags": {"redis.command":"GET"}}` corresponds to the following filter:

{% code title="SpanFilter" %}
```java
FilteringSpanListener.SpanFilter filter =
	new FilteringSpanListener.SpanFilter(
            null, // "domainName" is not checked
            "Redis", // Targeting only "Redis" operations
            null, // "operationName" is not checked
            new HashMap() {{
    	        put(RedisTags.COMMAND, "GET"); // Targeting only Redis "GET" commands
            }}
	);
```
{% endcode %}

The resulting filtering span listener would first filter the spans by looking at the `className` and the `tags` of the spans. If the given span has a different `className` and the `Redis` or the given span will not have the tag `redis.command` with the value `GET`. This means that the span will be rejected and won’t be sent to the `listener`. If it does have both properties, then the filterer accepts the span and `FilteringSpanListener` propagates the span to its listener parameter (which is a `ErrorInjectorSpanListener`).

### `SpanFilter`

Parameters include:

* **domainName**: The domain name value to compare with the given span’s domain name. The default value is `null`.
* **className**: The class name value to compare with the given span’s class name. The default value is `null`.
* **operationName**: The operation name value to compare with the given span’s operation name. The default value is `null`.
* **tags**: A dictionary that contains key-value tag pairs. `SpanFilter` checks that each of the key-value pairs in this dictionary also exists in the given span’s tags. If at least one of the key-value pairs does not exist in the span’s tags or exists with a different value, then the `SpanFilter` rejects the span. The default value is `null`.
