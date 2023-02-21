# Configuring Metrics for Java SDK

## Provided Metrics

Metric support provides all essential information regarding the performance of your Java Lambda functions, which translate to carefully calculated statistics. You can examine:

* Loaded class counts
* Memory usages
* Memory usages by pool
* CPU percentages
* GC CPU percentages
* GC counts
* GC durations
* Thread count

## Configuring Metrics

If you use Thundra’s Java layer (or custom runtime), the metric plugin is disabled by default; however, you can enable it by setting the `thundra_agent_lambda_metric_disable` environment variable to `false`.

Metrics are provided and reported periodically. With the following environment variables, the reporting period can be configured. If just one of the following periods reaches its configured limit, the metric is published.

*   `thundra_agent_metric_sample_sampler_timeAware_timeFreq`: Configures the time frequency in milliseconds to trigger metric reporting. Metrics are provided only once during the specified time period. The default value is `300,000` milliseconds (`5 minutes`).


* `thundra_agent_metric_sample_sampler_countAware_countFreq`: Configures the invocation count to trigger metric reporting. Metrics are provided only once for each of the specified invocation counts. The default value is `100`.
