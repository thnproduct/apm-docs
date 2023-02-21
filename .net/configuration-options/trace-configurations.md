# Configuring Traces for .NET SDK

## Configuring Trace Plugin

#### Disabling Request/Response Tracing for Lambda

Requests and responses are traced by default, but this can be disabled with environment variable configuration.

To disable tracing requests, set the `thundra_agent_lambda_trace_request_skip` environment variable to true.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_trace_request_skip: true
```
{% endcode %}

To disable tracing responses, set the `thundra_agent_lambda_trace_response_skip` environment variable to `true`.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_trace_response_skip: true
```
{% endcode %}

#### Disabling trace plugin using environment variables

{% code title="Configuration of Trace Plugin via Environment Variables" %}
```yaml
...
environment_variables:
    thundra_agent_lambda_trace_disable: true
...
```
{% endcode %}

## Trace Configuration of Automatically Supported Integrations

### Configuring AWS SDK Trace

#### Masking SQS Messages

You can mask SQS messages sent on the client’s side, which calls AWS SDK, by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_sqs_message_mask: true
```

#### Masking SNS Messages

You can mask SNS messages sent on the client’s side, which calls AWS SDK, by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_sns_message_mask: true
```

#### Masking DynamoDB Statements

You can mask DynamoDB statements sent on the client’s side, which calls AWS SDK, by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_dynamodb_statement_mask: true
```

#### Masking Lambda Payload

You can mask Lambda invocation payloads on the client’s side, which calls AWS SDK, by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_lambda_payload_mask: true
```

#### Unmasking Kinesis Records In Event

By default, incoming Kinesis records for a triggered Lambda are not traced. You can enable sending Kinesis records in a request by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_kinesis_record_unmask: true
```

#### Unmasking Firehose Records In Event

By default, incoming Firehose records for a triggered Lambda are not traced. You can enable sending Firehose records in a request by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_firehose_record_unmask: true
```

#### Enabling CloudWatch Log Messages In Event

By default, incoming CloudWatch logs for a triggered Lambda are not traced. You can enable sending CloudWatch logs in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_cloudwatchlog_request_enable: true
```

#### Disabling Sending AWS SDK Trace

Tracing AWS SDK calls is enabled by default, but can also be disabled through configuration if needed. You just need to set `thundra_agent_trace_instrument_integrations_aws_sdk_disable` to `true`.

### Configuring HTTP Trace

#### Masking HTTP Request Body

You can mask an HTTP request body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_http_body_mask: true
```

#### Disabling Sending HTTP Trace

Thundra traces HTTP calls by default. However, you may disable it using environment variables. You can set the `thundra_agent_trace_integrations_http_disable` environment variable to `true` for this purpose.
