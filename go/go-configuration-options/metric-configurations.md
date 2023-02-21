# Configuring Metrics for Go SDK

{% hint style="info" %}
The Thundra Go agent's metric plugin is disabled by default, and the Thundra console now receives metric data through CloudWatch integration. Older agent versions can continue to send metric data, but for optimal performance, it is recommended to use the latest version and add the integration. Read here to learn how to set up the integration.
{% endhint %}

## Thundra Go Metrics

Metric support provides essential information regarding the performance of your Go Lambda functions, which translate to carefully calculated statistics. This allows you to examine:

* Invocation Count
* Invocation Duration
* GC Metrics
* Heap Metrics
* CPU Metrics
* Disk Metrics
* Net Metrics
* Memory Metrics

## Configuring Metrics

You can configure the metric plugin using the following environment variables:

`thundra_agent_lambda_metric_disable`

As the metric plugin is **disabled** by default, you will need to set this to false if you want to enable the metric plugin. \
