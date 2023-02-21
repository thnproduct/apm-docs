# Configuring Traces for Java SDK

## Trace Configuration of Automatically Supported Integrations

### Configuring AWS SDK Trace

As you use AWS Lambda functions to deploy serverless, the entire AWS SDK becomes complementary to your application architecture. That means when developing and maintaining your application, it’s necessary to monitor how your Lambda functions are interacting with external AWS SDK services, such as DynamoDB, SQS, SNS, Kinesis, Firehose, S3, Lambda, Athena, etc. Thundra’s Java agent manages to integrate with AWS SDK, providing in-depth monitoring capabilities through trace spans. This functionality is enabled by default, but can also be disabled through configuration if needed.

#### Masking SQS Messages

You can mask SQS messages sent on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_sqs_message_mask: true
```

#### Masking SNS Messages

You can mask SNS messages sent on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_sns_message_mask: true
```

#### Masking DynamoDB Statements

You can mask DynamoDB statements on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_dynamodb_statement_mask: true
```

#### Masking Lambda Payload

You can mask Lambda invocation payloads on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_lambda_payload_mask: true
```

#### Masking API Gateway Request Body

You can mask a request body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_http_body_mask: true
```

#### Masking Athena Statements

You can mask Athena statements on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_athena_statement_mask: true
```

#### Unmasking Kinesis Records

By default, Kinesis records on the client’s side are not traced. You can enable tracing Kinesis records by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_kinesis_record_unmask: true
```

#### Unmasking Firehose Records

By default, Firehose records on the client’s side are not traced. You can enable tracing Firehose records by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_firehose_record_unmask: true
```

#### Disabling Sending AWS SDK Traces

Tracing AWS SDK calls is enabled by default, but can also be disabled through configuration if needed. You just need to set `thundra_agent_trace_integration_aws_disable` to `true.`

### Configuring Redis Traces

As fast access to data is a crucial attribute for several applications, you may use Redis clients (such as Redisson) with your Java Lambda functions in your application architectures. However, monitoring how your application interacts with Redis is quite the task, especially when clusters and nodes come into play. Thundra's Java agent integrates with various Java Redis clients to provide in-depth monitoring. Currently, the following Redis clients are supported:

* Jedis
* Lettuce
* Redisson

#### Masking Redis

You can mask Redis calls by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_redis_command_mask: true
```

#### Disabling Sending Redis Trace

Tracing Redis calls is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations via the Lambda environment variable. You just need to set `thundra_agent_trace_integration_redis_disable` to `true`.

### Configuring HTTP Trace

Monitoring HTTP requests is an important part of understanding how your system operates. Depending on how you implement your application, you may make several API calls; thus, you would need to be able to see how these interactions with your API are executed. Thundra's Java agent integrates with HTTP to trace HTTP requests over HTTP clients. Currently, the following HTTP clients are supported:

* Apache HTTP Client

#### Masking HTTP Request Body

You can mask a HTTP request body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_http_body_mask: true
```

#### Disabling Sending HTTP Trace

Thundra traces HTTP calls by default. However, you may disable it using environment variables. You can set the `thundra_agent_trace_integrations_http_disable` environment variable to `true` for this purpose.

### Configuring JDBC Trace

Data storage is an integral part of any application, and so monitoring how your Lambda functions in this context is of paramount importance. Thundra's Java agent traces query executions over JDBC `java.sql.Statements` and `java.sql.PreparedStatement`. Thundra is able to monitor JDBC responses for various database engines, which include the following:

* PostgreSQL
* MySQL
* MariaDB
* Microsoft SQL Server

#### Masking JDBC Queries&#x20;

You can mask JDBC queries on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_rdb_statement_mask: true
```

#### Disabling Sending JDBC Trace

Tracing JDBC queries is enabled by default, but can also be disabled if needed by setting the `thundra_agent_trace_integrations_rdb_disable` environment variable to `true`.

### Configuring Elasticsearch Trace

Thundra’s Java agent integrates with various Java Elasticsearch clients to provide in-depth monitoring. Currently, the following Elasticsearch clients are supported:

* High level Rest client (6.x versions)

#### Masking Elasticsearch Body&#x20;

You can mask an Elasticsearch body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_elasticsearch_body_mask: true
```

#### Disabling Sending Elasticsearch Trace

Tracing Elasticsearch responses is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations with the Lambda environment variable. You just need to set `thundra_agent_trace_integrations_elasticsearch_disable` to `true`.

### Configuring MongoDB Trace

Thundra’s Java agent integrates with various Java MongoDB clients to provide in-depth monitoring. Currently, the following MongoDB clients are supported:

* MongoDB Java Driver (3.1+ versions)

#### Masking MongoDB Commands&#x20;

You can mask MongoDB commands on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_mongodb_command_mask: true
```

#### Disabling Sending MongoDB Trace

Tracing MongoDB operations is enabled by default, but can also be disabled through configuration if needed. You can disable your integrations with the Lambda environment variable. You just need to set `thundra_agent_trace_integrations_mongodb_disable` to `true`.

## Configuring Trace Plugin

#### Disabling Request/Response Tracing for Lambda

By default, requests and responses are traced, but can be disabled with environment variable configuration.

To disable tracing requests, set the `thundra_agent_lambda_trace_request_skip` environment variable to `true`.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_trace_request_skip: true
```
{% endcode %}

To disable tracing responses, set the thundra\_agent\_lambda\_trace\_response\_skip environment variable to true.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_trace_response_skip: true
```
{% endcode %}

#### Unmasking Kinesis Records In Event

By default, incoming Kinesis records for a triggered Lambda are not traced. You can enable sending Kinesis records in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_kinesis_request_enable: true
```

#### Unmasking Firehose Records In Event

By default, incoming Firehose records for a triggered Lambda are not traced. You can enable sending Firehose records in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_kinesis_request_enable: true
```

#### Unmasking CloudWatch Log Messages In Event

By default, incoming CloudWatch logs for a triggered Lambda are not traced. You can enable sending CloudWatch logs in a request by setting the following environment variable:

```yaml
thundra_agent_lambda_trace_cloudwatchlog_request_enable: true

```

#### Customize Request Masking

With request tracing customization support, you can specify which fields of the request should be traced or how the request should be traced.

For example, if you only want to trace the `id` field of the `UserGetRequest`, you can customize the request masking behavior of trace support by overriding the trace plugin, as shown in the following example:

{% code title="UserGetHandler" %}
```java
....

import com.amazonaws.services.lambda.runtime.Context;

import io.thundra.agent.lambda.core.handler.request.LambdaRequestHandler;
import io.thundra.agent.lambda.core.handler.LambdaHandlerConfig;
import io.thundra.agent.lambda.core.handler.plugin.LambdaHandlerPlugin;

import io.thundra.agent.lambda.trace.handler.plugin.TraceLambdaHandlerPlugin;

...

public class UserGetHandler 
        implements LambdaRequestHandler<UserGetRequest, UserGetResponse> {

    ...

    @Override
    public LambdaHandlerConfig<UserGetRequest, UserGetResponse> getConfig() {
        return new LambdaHandlerConfig<UserGetRequest, UserGetResponse>() {
            @Override
            public List<LambdaHandlerPlugin<UserGetRequest, UserGetResponse>> getPlugins() {
                return Arrays.asList(
                        new TraceLambdaHandlerPlugin<UserGetRequest, UserGetResponse>() {
                            
                            ...

                            @Override
                            protected Object maskRequest(UserGetRequest request) {
                                return new HashMap<String, Object>() {{
                                   put("id", request.getId());
                                }};
                            }

                            ...
                            
                        });
            }
        };
    }

    ...

}
```
{% endcode %}

**Note:** This is also supported over `LambdaRequestStreamHandler`.

### Customize Response Masking

With response tracing customization support, you can specify which fields of the response should be traced or how the response should be traced.

For example, if you only want to trace the `id` field of the `User` in the `UserGetResponse`, you can customize response masking behavior of trace support by overriding the trace plugin, as shown in the following example:

{% code title="UserGetHandler" %}
```java
...

import io.thundra.agent.lambda.core.handler.request.LambdaRequestHandler;
import io.thundra.agent.lambda.core.handler.LambdaHandlerConfig;
import io.thundra.agent.lambda.core.handler.plugin.LambdaHandlerPlugin;

import io.thundra.agent.lambda.trace.handler.plugin.TraceLambdaHandlerPlugin;

...

public class UserGetHandler 
        implements LambdaRequestHandler<UserGetRequest, UserGetResponse> {

    ...

    @Override
    public LambdaHandlerConfig<UserGetRequest, UserGetResponse> getConfig() {
        return new LambdaHandlerConfig<UserGetRequest, UserGetResponse>() {
            @Override
            public List<LambdaHandlerPlugin<UserGetRequest, UserGetResponse>> getPlugins() {
                return Arrays.asList(
                        new TraceLambdaHandlerPlugin<UserGetRequest, UserGetResponse>() {
                            
                            ...

                            @Override
                            protected Object maskResponse(UserGetResponse response) {
                                return new HashMap<String, Object>() {{
                                    put("id", response.getUser() != null ? response.getUser().getId() : null);
                                }};
                            }
                            
                            ...

                        });
            }
        };
    }

    ...

}
```
{% endcode %}

**Note:** This is also supported over `LambdaRequestStreamHandler`.
