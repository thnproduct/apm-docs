# Configuring Metrics for Node.js

## Thundra Node.js Metrics

Metric support provides essential information regarding the performance of your Node.js Lambda functions, which translate to carefully calculated statistics. You can examine:

* Invocation Counts
* Invocation Durations
* Memory Usages
* Process Memory Usages
* CPU Percentages
* Disk IO
* Thread Count

## Configuring Metrics

By simply importing `thundra` from `@thundra/core`, all the metrics mentioned above are ready to be collected; however, metrics are disabled by default. You need to enable them programmatically or through environment variables.

### Enabling Sending Metrics

Metric monitoring can be enabled by setting the relevant configuration parameters. These parameters include the `thundra_agent_lambda_metric_disable` environment variable and the `disable_metric` object initialization parameter.

#### Enabling metric programmatically on agent initialization

{% code title="Programmatic Metric Configuration" %}
```javascript
const thundra = require("@thundra/core")({ 
  metricConfig: {
    enabled: true,
  },
});
```
{% endcode %}

#### Enabling metric using environment variables

{% code title="Metric Configuration via Environment Variables" %}
```yaml
thundra_apiKey: ${self:custom.thundraApiKey}
thundra_agent_lambda_metric_disable: false
```
{% endcode %}

