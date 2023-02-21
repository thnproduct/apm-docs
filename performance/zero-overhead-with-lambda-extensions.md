# Zero Overhead with Lambda Extensions

For AWS Lambda monitoring, there's an inevitable overhead due to the transmission of the data to the collectors. Thundra already solves this problem by [asynchronous monitoring ](https://docs.thundra.io/performance/zero-overhead-with-asynchronous-monitoring)by gathering logs from CW. With the preview announcement of Lambda Extensions, Thundra provides an alternative way of transmitting observability data to our collectors.&#x20;

By default, Thundra agent collects telemetry data during invocation and send them at the end of invocation to the Thundra collector endpoint (communicates with regional collector endpoint `${region}.collector.thundra.io` to minimize network RTT). Of course this adds extra latency to the invocation duration around **3-4** milliseconds on average.

![](<../.gitbook/assets/Thundra Collector Latency.png>)

Thundra Lambda extension is a kind of companion process which provides asynchronous telemetry data (traces, metrics, logs) reporting functionality to the Thundra agents. To do that, Thundra extension starts a local collector and Thundra agent sends collected data to the local collector endpoint without any network delay. Then Thundra extension sends buffered data to the real collector asynchronously along with the execution of the invocation. So there will be zero network delay added by Thundra agent. Additionally, remaining buffered telemetry data is flushed on shutdown of the Lambda container.

## **How to Setup Thundra Lambda Extension?**

* Thundra Lambda extension is also an extension to Thundra Lambda agent running along with your application Our Lambda extension is supported every runtime that Thundra has the agent for - Node.js, Python, Java, Go, .NET. In order to use our extension, the Thundra agent must be [integrated](https://docs.thundra.io/) into your Lambda application first.
* Then, the Thundra agent must be configured to use the local collector as the data transmission point, which is provided by the Thundra Lambda extension, instead of the remote collector. The configuration can be done easily through environment variables by setting `THUNDRA_AGENT_REPORT_REST_LOCAL` to `true`

#### Add Thundra Lambda Extension with AWS Lambda layer

Thundra Lambda extension is published as an AWS Lambda layer which includes extension implementation. So it can be added as follows:

`arn:aws:lambda:${region}:269863060030:layer:thundra-lambda-extension:3`

#### Add Thundra Lambda Extension with Container Images

Thundra Lambda extension is available as container images as well. So it can be added as follows:

`FROM thundraio/thundra-lambda-extension:LATEST`

For Java runtime, an example Dockerfile configuration:

{% code title="Dockerfile" %}
```yaml
FROM thundraio/thundra-lambda-container-base-java8:LATEST
FROM thundraio/thundra-lambda-extension:LATEST AS ext

# copy Thundra Lambda extension to /opt 
WORKDIR /opt
COPY --from=ext /opt/ .

# your function code path
ARG FUNCTION_CODE_PATH="target/sample-app.jar"

# task directory
ARG TASK_DIR="/var/task"

# create task directory
RUN mkdir -p ${TASK_DIR}

# copy your function code to the task directory
COPY ${FUNCTION_CODE_PATH} ${TASK_DIR}

# Set the CMD to your handler
CMD ["io.thundra.container.sample.Handler"]
```
{% endcode %}

&#x20;\








&#x20;&#x20;
