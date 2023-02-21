# Configuring Metrics for .NET Agent

{% hint style="danger" %}
The Thundra .NET agent's metric plugin is disabled on version 1.5.0. The Thundra console now receives metric data through CloudWatch integration. Older agent versions can continue to send metric data, but it is recommended to use the latest version and add the integration. Read here to learn how to set up CloudWatch integration.
{% endhint %}

The metric plugin provides essential information regarding the performance of your .NET Lambda functions, which translate to carefully calculated statistics. It lets you examine:

* Invocation Count
* Invocation Duration
* CPU Usage
* Memory Usage

## Configuring Metrics

The configuration of .NET metrics can be done in three different ways: programmatic configurations, configuration via environment variables, and user configuration files. By configuring metrics, you can choose to either enable or disable them. Metric data is disabled by default. When you enable metrics, they will be recorded in your functions’ metrics tab.

### Enabling Metric Plugins

Metric monitoring is disabled by default, but can be enabled by setting the `thundra_agent_lambda_metric_disable` environment variable to false.  \
