# Configuring Traces for Python SDK

## Instrumentation of Chalice Applications

Instrumentation of Chalice applications is completed automatically, so all you need to do is initialize the Thundra agent. Just add the lines shown below to your code:

```python
from thundra.thundra_agent import Thundra

# Pass configuration parameters here or add as environment variables
# to your chalice config.json
thundra = Thundra()
```

An example is shown below:

```python
from chalice import Chalice
from thundra.thundra_agent import Thundra

# Pass configuration parameters here or add as environment variables
# to your chalice config.json
thundra = Thundra()

app = Chalice(app_name='test')

@app.route('/')
def index():
    return {'hello': 'world'}
```

### Configuring Environment Variables For Chalice

Chalice deployment is facilitated by setting Thundra’s environment variables in your .chalice/config.json file. Deploying your Chalice application will set these variables and allow Thundra to monitor your functions.

You can add environment variables either globally or for a specific stage, as shown below in your .chalice/config.json:

```javascript
{
  "version": "2.0",
  "app_name": "test",

  "environment_variables": {
    "thundra_apiKey": <your api key here>
  },
  "stages": {
    "dev": {
      "environment_variables": {
        "thundra_apiKey": <your api key here>
      },
      "api_gateway_stage": "api"
    }
  }
}
```

### Disabling Chalice Instrumentation

Tracing route handler functions created with Chalice is enabled by default, but it can also be disabled through configuration if needed. You just need to set the `thundra_agent_lambda_trace_integrations_chalice_disable` environment variable to `true`.

## Trace Configuration of Automatically Supported Integrations

### Configuring AWS SDK Trace

#### Masking SQS Messages

You can mask SQS messages sent on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_aws_sqs_message_mask: true
```

#### Masking SNS Messages

You can mask SNS messages sent on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_aws_sns_message_mask: true
```

#### Masking DynamoDB Statements

You can mask DynamoDB statements sent on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_aws_dynamodb_statement_mask: true
```

#### Masking Lambda Payload

You can mask Lambda invocation payloads on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_aws_lambda_payload_mask: true
```

#### Masking API Gateway Request Body

You can mask a request body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_http_body_mask: true
```

#### Masking Athena Statements

You can mask Athena statements on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_aws_athena_statement_mask: true
```

#### Unmasking Kinesis Records In Event

Incoming Kinesis records at a triggered Lambda are not traced by default. You can enable sending Kinesis records in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_kinesis_request_enable: true
```

#### Unmasking Firehose Records In Event

Incoming Firehose records at a triggered Lambda are not traced by default. You can enable sending Firehose records in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_kinesis_request_enable: true
```

#### Unmasking CloudWatch Log Messages In Event

Incoming CloudWatch logs at a triggered Lambda are not traced by default. You can enable sending CloudWatch logs in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_cloudwatchlog_request_enable: true
```

#### Disabling Sending AWS SDK Trace

Tracing AWS SDK calls is enabled by default, but can also be disabled through configuration if needed. You just need to set `thundra_agent_lambda_trace_integration_aws_disable` to `true`.

### AWS Step Functions

You can see distributed traces of step functions by setting the following environment variable:

```yaml
thundra_agent_lambda_aws_stepfunctions: true
```

This variable enables step function trace link injection to the response. This configuration is disabled by default.

### AWS AppSync

You can see distributed traces of appsync by setting the following environment variable:

```html
thundra_agent_lambda_aws_appsync: true
```

This variable enables appsync trace link injection to the response. This configuration is disabled by default.

### Configuring Redis Trace

#### Masking Redis

You can mask Redis calls by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_redis_command_mask: true
```

#### Disabling Sending Redis Trace

Tracing Redis calls is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations via the Lambda environment variable. You just need to set `thundra_agent_lambda_trace_integration_redis_disable` to `true`.

### Configuring HTTP Trace

#### Masking HTTP Request Body

You can mask an http request body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_http_body_mask: true
```

#### Disabling Sending HTTP Trace

Thundra traces HTTP calls by default. However, you may disable it using environment variables. You can set the `thundra_agent_lambda_trace_integrations_http_disable` environment variable to `true` for this purpose.

#### Configuring Http Errors

Tracing errors by status code on http/https response can be configured by setting  `thundra_agent_trace_integrations_http_error_status_code_min` environment variable. Default value for this environment variable is 400, so the responses with 4xx and 5xx status codes are treated as erroneous and the error is traced by default.

As an example, if you only want to see the 5xx errors and not 4xx the configuration can be like below:

```yaml
thundra_agent_trace_integrations_http_error_status_code_min: 500 
```

### Configuring MySQL Trace

#### Masking MySQL Queries&#x20;

You can mask MySQL queries on the client’s side by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_rdb_statement_mask: true
```

#### Disabling Sending MySQL Trace

Tracing MySQL queries is enabled by default, but can also be disabled if needed by setting the `thundra_agent_lambda_trace_integrations_rdb_disable` environment variable to `true`.

### Configuring PostgreSQL Trace

#### Masking PostgreSQL Queries&#x20;

You can mask PostgreSQL queries on the client’s side by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_rdb_statement_mask: true
```

#### Disabling Sending PostgreSQL Trace

Tracing PostgreSQL queries is enabled by default, but can also be disabled if needed by setting the `thundra_agent_lambda_trace_integrations_rdb_disable` environment variable to `true`.

### Configuring Elasticsearch Trace

#### Masking Elasticsearch Body&#x20;

You can mask an Elasticsearch body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_elasticsearch_body_mask: true
```

#### Disabling Sending Elasticsearch Trace

Tracing Elasticsearch responses is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations via the Lambda environment variable. You just need to set `thundra_agent_lambda_trace_integrations_elasticsearch_disable` to `true`.

### Configuring SQLAlchemy Trace

#### Disabling Sending SQLAlchemy Trace

Tracing SQLAlchemy operations is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations via the Lambda environment variable. You just need to set `thundra_agent_lambda_trace_integrations_sqlalchemy_disable` to `true`.

### Configuring MongoDB Trace

#### Masking MongoDB Commands&#x20;

You can mask MongoDB commands on the client’s side by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_integrations_mongodb_command_mask: true
```

#### Disabling Sending MongoDB Trace

Tracing MongoDB operations is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations via the Lambda environment variable. You just need to set `thundra_agent_lambda_trace_integrations_mongodb_disable` to `true`.

## Configuring Trace Plugin

#### Disabling Request/Response Tracing for Lambda

Requests and responses are traced by default, but can be disabled with environment variable configuration.

To disable tracing requests, set the `thundra_agent_lambda_trace_request_skip` environment variable to `true`.

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

#### Disabling the Trace Plugin Programmatically on Agent Initialization

{% code title="Programmatic Configuration" %}
```python
thundra = Thundra(api_key=’my_api_key’, disable_trace=True)
```
{% endcode %}

#### Disabling the Trace Plugin Using Environment Variables

{% code title="Configuration of Trace Plugin via Environment Variables" %}
```yaml
thundra_apiKey: ${self:custom.thundraApiKey}
thundra_agent_lambda_trace_disable: true
```
{% endcode %}





