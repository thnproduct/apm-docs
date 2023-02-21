# Configuring Metrics for Python SDK

## Thundra Python Metrics

Metric support provides essential information regarding the performance of your Python Lambda functions, which translate to carefully calculated statistics. You can examine:

* Memory Metrics
* CPU Metrics
* GC Count
* Thread Count

## Configuring Metrics

By simply importing `Thundra` from `thundra.thundra_agent`, all metrics mentioned above are collected and are presented to you within the Thundra Console.&#x20;

### Disabling Sending Metrics

By simply importing Thundra from `thundra.thundra_agent`, all the metrics mentioned above are collected and presented to you within the Thundra console. However, metric monitoring is disabled by default. You can enable them by setting the relevant configuration parameters, which include the `thundra_agent_lambda_metric_disable` environment variable and the `disable_metric` object initialization parameter.

#### Enabling metric programmatically on agent initialization

{% code title="Programmatic Metric Configuration" %}
```python
thundra = Thundra(api_key=’my_api_key’, disable_metric=False)
```
{% endcode %}

#### Enabling metric using environment variables

{% code title="Metric Configuration via Environment Variables" %}
```yaml
thundra_apiKey: ${self:custom.thundraApiKey}
thundra_agent_lambda_metric_disable: false
```
{% endcode %}

