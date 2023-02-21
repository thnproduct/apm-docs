# Sampling in Java SDK

You can reduce the amount of data that the Java agent sends to the Thundra Web Console by enabling sampling. You can either add built-in sampling rules or provide a custom sampler and implement the sampling logic by yourself. Either option allows you to sample **Trace**, **Metric**, and/or **Log** data using the **Sampling API**.

## Sampling API

### Sampler

This interface decides whether or not sampling should be instrumented. You can create your own custom sampler by implementing `io.thundra.agent.core.sample.Sampler`, which is the interface where data (such as trace, metric, log, or other data, depending on what your sampler is designed to use) that makes sampling decisions is located.

{% code title="Sampler" %}
```java
public interface Sampler<A> {

    /**
     * Checks whether or not sampling should be done.
     *
     * @param arg to be used for sampling decision
     * @return <code>true</code> if sampling should be done,
     *         <code>false</code> otherwise
     */
    boolean isSampled(A arg);

}
```
{% endcode %}

### `CountAwareSampler`

This sampler is a count-based implementation, which allows sampling for every specified count period. The default count frequency is `100`, but it can be configured using the `thundra_agent_sampler_countAware_countFreq` environment variable.

As an example, if the count frequency is `500`, data will be sampled with every `500` Lambda invocations. Here is the configuration:

{% code title="CountAwareSampler" %}
```java
import io.thundra.agent.core.sample.impl.CountAwareSampler;

...

// Sample for every 500 call
CountAwareSampler countAwareSampler = new CountAwareSampler(500);
```
{% endcode %}

### `TimeAwareSampler`

This is a time-based implementation, which allows sampling for every specified time period. The default time frequency is `300,000` milliseconds (`5` minutes), but it can be configured using the `thundra_agent_sampler_timeAware_timeFreq` environment variable.

As an example, if the time frequency is `60,000`, data will be sampled every `60` seconds (`10` minutes) within consecutive Lambda invocations. Here is the configuration:

{% code title="TimeAwareSampler" %}
```java
import io.thundra.agent.core.sample.impl.TimeAwareSampler;

...

// Sample for every 60000 milliseconds (60 seconds = 1 minute)
TimeAwareSampler timeAwareSampler = new TimeAwareSampler(60000);
```
{% endcode %}

### `CompositeSampler`

This implementation composes multiple samplers with a specified composition operation (`AND` or `OR`).

{% code title="CompositeSampler" %}
```java
import io.thundra.agent.core.sample.impl.CountAwareSampler;
import io.thundra.agent.core.sample.impl.TimeAwareSampler;
import io.thundra.agent.core.sample.impl.CompositeSampler;

import static io.thundra.agent.core.sample.impl.CompositeSampler.SamplerCompositionOperator.OR;

...
  
CountAwareSampler countAwareSampler = new CountAwareSampler(500);
TimeAwareSampler timeAwareSampler = new TimeAwareSampler(1000);
// Sample at every 500 call or 60000 milliseconds (10 minutes), whichever comes first
CompositeSampler compositeSampler = new CompositeSampler(OR, countAwareSampler, timeAwareSampler);
```
{% endcode %}

## Trace Sampling

Samplers can be set using the setTraceSampler static method of the `io.thundra.agent.trace.TraceSupport` class, which samples traces (spans).

#### Sampling Target

For tracing, sampling is only applied to the root span (not the other internal spans) of the invocation that represents the actual Lambda invocation. You can also choose to have all of the spans in the invocation automatically sampled if the root span is sampled.

### `DurationAwareThundraSpanSampler`

This implementation samples Thundra spans if their duration in milliseconds meets or exceeds the duration you have set. For example, say you want to send trace data only if the duration of the Lambda invocation is longer than `1` second (`1000` milliseconds). Here is the configuration:

{% code title="DurationAwareThundraSpanSampler" %}
```java
import io.thundra.agent.trace.sample.impl.DurationAwareThundraSpanSampler;
import io.thundra.agent.trace.TraceSupport;

...
  
// Sample only if the duration of the invocation (root span) is longer than 1 seconds  
DurationAwareThundraSpanSampler durationAwareSampler = 
  	new DurationAwareThundraSpanSampler(1000);

// Set sampler to be used for tracing
TraceSupport.setSampler(durationAwareSampler);
```
{% endcode %}

### `ErrorAwareThundraSpanSampler`

This implementation samples Thundra spans if they are erroneous (tagged by error). For example, say you want to send trace data only if the Lambda execution fails with an error:

{% code title="ErrorAwareThundraSpanSampler" %}
```java
import io.thundra.agent.trace.sample.impl.ErrorAwareThundraSpanSampler;
import io.thundra.agent.trace.TraceSupport;

...
  
// Sample only if the invocation (root span) fails with an error  
ErrorAwareThundraSpanSampler errornAwareSampler = 
  	new ErrorAwareThundraSpanSampler();

// Set sampler to be used for tracing
TraceSupport.setSampler(errornAwareSampler);
```
{% endcode %}

## Metric Sampling

Samplers can be set using the `setMetricSampler` static method of the `io.thundra.agent.metric.MetricSupport` class, which is used for sampling metrics (stats).

#### Default Metric Sampling

By default, metrics are sampled every `100` invocations or `5` minutes, whichever comes first. Default invocation count and time frequencies can be configured with the `thundra_agent_sampler_countAware_countFreq` and `thundra_agent_sampler_timeAware_timeFreq` environment variables. You can also set your custom sampler over MetricSampler, as described above.

## Log Sampling

Samplers can be set using the `setLogSampler` static method of the `io.thundra.agent.log.LogSupport` class, which is used for sampling logs.
