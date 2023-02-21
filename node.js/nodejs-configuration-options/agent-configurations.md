# Configuring Node.js SDK

### Enabling Debug Logs&#x20;

Debug logs can be enabled by setting the `thundra_agent_lambda_debug_enable` environment variable to `true`.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_debug_enable: true
```
{% endcode %}

### Configuring Thundra API URL

To change the URL that Thundra’s agents send reports to, set the `thundra_agent_lambda_report_rest_baseUrl` environment variable to `true`. Note that normally you don't need to set this environment variable to a URL because the default value that agents use will work automatically. The one exception to this is if you are forwarding Thundra data to our partner environments (Splunk, Honeycomb).&#x20;

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_report_rest_baseUrl: <report-url>
```
{% endcode %}

{% hint style="info" %}
Path `/monitoring-data` is added to the URL automatically. For instance, in the default case (`https://api.thundra.io/v1`) the agent makes an `https` post request to the path `https://api.thundra.io/v1/monitoring-data`.
{% endhint %}

### Enabling Warmup

To enable warmup and, by extension, reduce cold starts, set the `thundra_agent_lambda_warmup_warmupAware` environment variable to `true`. However, even after setting this up, there are additional steps you will need to take. Check out the [How to Warmup](https://docs.thundra.io/performance/dealing-with-cold-starts) page for more information.

### Configuring Application Stage

To specify the profile from which monitoring data was collected, you will need to configure the application stage. Profiles are used to represent different environments, such as "development," "staging," "production," etc.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_application_stage: <application-stage>
```
{% endcode %}

### Configuring Function Timeout Margin

This configuration specifies how much time is needed to send a report before a Lambda times out. You should change this variable if the `timeout_margin` is not enough and you don’t see your timed-out functions on the Thundra Console. The default value is 200 ms.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_timeout_margin: <timeout-margin-value-in-ms>
```
{% endcode %}

### Configuring Apdex Score

The Apdex score helps you understand the performance of your Lambda functions in terms of latency. In order for Thundra to calculate the Apdex score, you should add new environment variables to your functions. See the values below.

\- `thundra_agent_lambda_application_tag_apdex_target_time`: Specifies the time in milliseconds, which gives the end user the response time of the Lambda function.&#x20;

\- `thundra_agent_lambda_application_tag_apdex_tolerating_ratio`: Specifies the ratio between the Apdex target time and tolerating time. This configuration is optional. Default value is 4.&#x20;

\- `thundra_agent_lambda_application_tag_apdex_tolerating_time`: Specifies the Lambda function’s maximum response time threshold that can be tolerated by the end user. This is optional.
