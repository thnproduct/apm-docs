# Configuring Go SDK

After you integrate Thundra’s agent into your application, you can configure settings to decide which properties will be enabled on your agent. Environment variables that you can use to configure your agent are listed below.

## Environment Variables

| Name                                                       | Type   | Default Value                                          |
| ---------------------------------------------------------- | ------ | ------------------------------------------------------ |
| thundra\_apiKey                                            | string | -                                                      |
| thundra\_agent\_lambda\_disable                            | bool   | false                                                  |
| thundra\_agent\_lambda\_timeout\_margin                    | number | 200                                                    |
| thundra\_agent\_lambda\_report\_rest\_baseUrl              | string | [https://api.thundra.io/v1](https://api.thundra.io/v1) |
| thundra\_agent\_lambda\_report\_cloudwatch\_enable         | bool   | false                                                  |
| thundra\_agent\_lambda\_trace\_disable                     | bool   | false                                                  |
| thundra\_agent\_lambda\_metric\_disable                    | bool   | true                                                   |
| thundra\_agent\_lambda\_log\_disable                       | bool   | true                                                   |
| thundra\_log\_logLevel                                     | string | TRACE                                                  |
| thundra\_agent\_lambda\_trace\_request\_disable            | bool   | false                                                  |
| thundra\_agent\_lambda\_trace\_response\_disable           | bool   | false                                                  |
| thundra\_agent\_lambda\_report\_rest\_trustAllCertificates | bool   | false                                                  |
| thundra\_lambda\_debug\_enable                             | bool   | false                                                  |
| thundra\_agent\_lambda\_warmup\_warmupAware                | bool   | false                                                  |
|                                                            |        |                                                        |

#### Thundra Api Key Configuration

{% code title="Thundra Api Key Configuration" %}
```
thundra_apiKey
```
{% endcode %}

Specifies the API key that monitoring data will be sent through. You won't be able to publish your data without setting an API key. You can generate an API key at [Thundra](https://console.thundra.io/), visit [Projects](https://docs.thundra.io/thundra-web-console/profile/projects-page) page to learn more.

### Lambda Agent Configuration

{% code title="Lambda Agent Configuration" %}
```
thundra_agent_lambda_disable
```
{% endcode %}

Set this configuration to `true` if you want to disable Thundra.

#### Lambda Agent Trace Configuration

{% code title="Lambda Agent Trace Configturation" %}
```
thundra_agent_lambda_trace_disable
```
{% endcode %}

Set this configuration to `true` if you want to disable the trace plugin.

#### Lambda Agent Log Configuration

{% hint style="info" %}
The Thundra Go agent's log plugin is disabled by default.
{% endhint %}

{% code title="Lambda Agent Log Configuration" %}
```
thundra_agent_lambda_log_disable
```
{% endcode %}

Set this configuration to `false` if you want to enable the log plugin.

#### Lambda Agent Log Level

{% code title="Lamba Agent Log Level" %}
```
thundra_log_logLevel
```
{% endcode %}

Log level can be set with one of the following values:

`TRACE`,`DEBUG`,`INFO`,`WARN`,`ERROR`,`NONE`

#### **Lambda Agent Metric Configuration**

{% hint style="info" %}
By default, the Thundra Go agent's metric plugin is disabled.
{% endhint %}

{% code title="Lambda Agent Metric Configuration" %}
```
thundra_agent_lambda_metric_disable
```
{% endcode %}

Set this configuration to `false` if you want to enable the metric plugin.

**Lambda Agent CloudWatch Configuration**

{% code title="Lambda Agent CloudWatch Configuration" %}
```
thundra_agent_lambda_report_cloudwatch_enable
```
{% endcode %}

Set this configuration to `true` if you want to enable asynchronous monitoring. Note that this is just one step you need to take for this process: Check out our [How to Setup Async Monitoring](https://docs.thundra.io/docs/how-to-setup-async-monitoring-v2) page for more information.

### Lambda Agent WarmUp Configuration

{% code title="Lambda Agent WarmUp Configuration" %}
```
thundra_agent_lambda_warmup_warmupAware
```
{% endcode %}

Set this configuration to `true` if you want to enable warming up and reduce cold starts. Note that this is just one step you need to take for this process: Check out our [How to Warmup](https://docs.thundra.io/performance/dealing-with-cold-starts) page for more information.

### **Lambda Agent Trace Request Configuration**

{% code title="Lambda Agent Trace Request Configration" %}
```
thundra_agent_lambda_trace_request_skip
```
{% endcode %}

Set this configuration to `true` if you want to disable monitoring requests.

### **Lambda Agent Trace Response Configuration**

{% code title="Lambda Agent Trace Response Configuration" %}
```
thundra_agent_lambda_trace_response_skip
```
{% endcode %}

Set this configuration to `true` if you want to disable monitoring responses.

### **Thundra Agent Report EndPoint Configuration**

{% code title="Thundra Agent Report EndPoint Configuration" %}
```
thundra_agent_lambda_report_rest_baseUrl
```
{% endcode %}

Set this configuration if you want to change the URL to which the Thundra agent sends reports. If you are not forwarding Thundra data to our partner environments (Splunk, Honeycomb), then it is unnecessary to set this environment variable to a URL.

Note: path `/monitoring-data` is added to the URL automatically. For instance, in the default case (`https://api.thundra.io/v1`), the agent makes an HTTPS post request to the path `https://api.thundra.io/v1/monitoring-data`.

### **Thundra Agent Timeout Margin Configuration**

{% code title="Thundra Agent Timeout Margin Configuration" %}
```
thundra_agent_lambda_timeout_margin
```
{% endcode %}

This specifies how much time is needed to send a report before a Lambda times out. You should change this variable if the timeout\_margin is not enough and you do not see your timeout functions on the Thundra console. The default value is 200 ms.

### **Thundra Agent Certificate Configuration**

{% code title="Thundra Agent Certificate Configuration" %}
```
thundra_agent_lambda_report_rest_trustAllCertificates
```
{% endcode %}

When dealing with HTTPS and HTTP with asynchronous functions, you can enable this configuration by setting it to true to interact with HTTPS or HTTP regardless.

\
\
\
\
\
\


### &#x20;
