# Configuring Traces for Node.js SDK

## Trace Configuration of Automatically Supported Integrations

### Disabling the integrations

To disable an integration, you can use the `thundra_agent_trace_integrations_disable` environment variable. Set this environment variable's value to a string that includes a comma (,) in order to separate integration names.

For example, to disable `redis` and the `http` integration, you need to use the following environment variable and value: `thundra_agent_trace_integrations_disable: redis,http`



### Configuring AWS SDK Trace&#x20;

#### Masking SQS Messages

You can mask sent SQS messages on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_sqs_message_mask: true
```

#### Masking SNS Messages

You can mask sent SNS messages on the client’s side, which calls AWS SDK by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_aws_sns_message_mask: true
```

#### Masking DynamoDB Statements

You can mask sent DynamoDB statements on the client’s side, which calls AWS SDK by setting the following environment variable:

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

#### Disabling AWS SDK Trace

Tracing AWS SDK calls is enabled by default, but can also be disabled through configuration if needed. You just need to add the `aws` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here: `thundra_agent_trace_integrations_disable: aws`

### AWS Step Functions

You can see distributed traces of step functions by setting the following environment variable:

```yaml
thundra_agent_lambda_aws_stepfunctions: true
```

This variable enables step function trace link injection to the response. This configuration is disabled by default.

### AWS AppSync

You can see distributed traces of AppSync by setting the following environment variable:

```yaml
thundra_agent_lambda_aws_appsync: true
```

This variable enables AppSync trace link injection to the response. This configuration is disabled by default.

### Configuring Redis Trace

#### Masking Redis

You can mask Redis calls by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_redis_command_mask: true
```

#### Disabling Redis Trace

Tracing Redis calls is enabled by default, but can also be disabled  through configuration if needed. You just need to add the `redis` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: redis`&#x20;

### Configuring IORedis Trace

#### Masking IORedis

You can mask IORedis calls by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_redis_command_mask: true
```

#### Disabling IORedis Trace

Tracing IORedis calls is enabled by default, but can also be disabled  through configuration if needed. You just need to add the `ioredis` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: ioredis`&#x20;

### Configuring HTTP/HTTPS Trace

#### Masking HTTP Request Body

You can mask the http request body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_http_body_mask: true
```

#### Disabling HTTP/HTTPS Trace

Tracing HTTP/HTTPS calls is enabled by default, but can also be disabled through configuration if needed. You just need to add the `http` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: http`&#x20;

#### Disabling 4XX/5XX Errors

Tracing 4XX/5XX errors on http/https response is enabled by default, but also be disabled through configuration by adding following environment variables.

```yaml
thundra_agent_trace_integrations_http_error_on4xx_disable: true #for 4XX errors
thundra_agent_trace_integrations_http_error_on5xx_disable: true #for 5XX errors
```

### Configuring MySQL/MySQL2 Trace

#### Masking MySQL/MySQL2 Queries&#x20;

You can mask MySQL/MySQL2 queries on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_rdb_statement_mask: true
```

#### Disabling MySQL/MySQL2 Trace

Tracing MySQL/MySQL2 calls is enabled by default, but can also be disabled through configuration if needed. You just need to add the mysql or `mysql2` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: mysql,mysql2`&#x20;

### Configuring PostgreSQL Trace

#### Masking PostgreSQL Queries&#x20;

You can mask PostgreSQL queries on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_rdb_statement_mask: true
```

#### Disabling PostgreSQL Trace

Tracing PostgreSQL calls is enabled by default, but can also be disabled through configuration if needed. You just need to add the `pg` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: pg`&#x20;

### Configuring Elasticsearch Trace

#### Masking Elasticsearch Body&#x20;

You can mask the Elasticsearch body on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_elasticsearch_body_mask: true
```

#### Disabling Elasticsearch Trace

Tracing Elasticsearch calls is enabled by default, but can also be disabled through configuration if needed. You just need to add the `es` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: es`&#x20;

### Configuring MongoDB Trace

#### Masking MongoDB Commands&#x20;

You can mask MongoDB commands on the client’s side by setting the following environment variable:

```yaml
thundra_agent_trace_integrations_mongodb_command_mask: true
```

#### Disabling MongoDB Trace

Tracing MongoDB calls is enabled by default, but can also be disabled through configuration if needed. You just need to add the `mongodb` value to the array string for the `thundra_agent_trace_integrations_disable` environment variable as shown here:

`thundra_agent_trace_integrations_disable: mongodb`&#x20;

### Configuring Trace Plugin

#### Masking Request

Requests can be masked by setting the maskRequest function into traceConfig.

```yaml
const thundra = require("@thundra/core");
const config = {
  traceConfig: {
    maskRequest: (event) => {
      // Mask the event
      event.password = "???";
      return event;
    }
  }
};
exports.handler = thundra(config)((event, context, callback) => {
  callback(null, {
    msg: 'hello world'
  });
});
```

#### Masking Response

Responses can be masked by setting the maskResponse function into traceConfig.

```yaml
const thundra = require("@thundra/core");
const config = {
  traceConfig: {
    maskResponse: (response) => {
      // Mask the response
      response.msg = "???";
      return response;
    }
  }
};
exports.handler = thundra(config)((event, context, callback) => {
  callback(null, {
    msg: 'hello world'
  });
});
```



#### Disabling Request/Response Tracing for Lambda

Requests and responses are traced by default, but can be disabled with environment variable configuration.

To disable tracing requests, set the `thundra_agent_lambda_trace_request_skip` environment variable to `true`.

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_trace_request_skip: true
```
{% endcode %}

To disable tracing response set the `thundra_agent_lambda_trace_response_skip`environment variable to `true`

{% code title="Configuration via Environment Variable" %}
```yaml
thundra_agent_lambda_trace_response_skip: true
```
{% endcode %}

#### Disabling trace plugin programmatically on agent initialization

{% code title="Disabling trace plugin programatically" %}
```javascript
const thundra = require("@thundra/core")({ 
  traceConfig: {
    enabled: false,
  },
});
```
{% endcode %}

#### Disabling trace plugin using environment variables

{% code title="Configuration of Trace Plugin via Environment Variables" %}
```yaml
thundra_agent_trace_disable: true
```
{% endcode %}

### Configuring Google Pub / Sub

#### Publish

```javascript
const { PubSub } = require('@google-cloud/pubsub');

const projectId = 'your_google_cloud_project_id';
const topicName = 'your_google_cloud_pubsub_topic';

const pubsub = new PubSub({ projectId });

/* 
* if topic allready exists 
* const topic = await pubsub.topic(topicName)
**/
const topic = await pubsub.createTopic(topicName);

const date = new Date().toString();
const dataBuffer = Buffer.from(JSON.stringify({date}));

const result = await topic.publishMessage({ data: dataBuffer });
```

**Subscription**

**Asynchronous Pull**

```javascript
const thundra = require("@thundra/core");
thundra.init();

const { PubSub, Subscription } = require('@google-cloud/pubsub');

const projectId = 'your_google_cloud_project_id';
const topicName = 'your_google_cloud_pubsub_topic';
const subscriptionName = 'your_google_cloud_pubsup_subscription_name';

const pubsub = new PubSub({ projectId });

(async() => {
    
    /* 
    * if subscription allready exists 
    * const subscription = pubsub.subscription(subscriptionName);
    **/
    const [subscription] = await pubsub.topic(topicName).createSubscription(subscriptionName);
    
    const messageHandler = message => {
      try {
        ...
        message.ack();
      } catch (err) {
        ...
        message.nack();
      }
    };
    
    subscription.on(`message`, messageHandler);
})().catch(error => console.log(error));
```

**Synchronous Pull**

```javascript
const { v1 } = require('@google-cloud/pubsub');

const subClient = new v1.SubscriberClient();

const projectId = 'your_google_cloud_project_id';
const subscriptionName = 'your_google_cloud_pubsup_subscription_name';

const formattedSubscription = subClient.subscriptionPath(
  projectId,
  subscriptionName
);

const request = {
  subscription: formattedSubscription,
  maxMessages: 10,
};

...

const result = await subClient.pull(request);
const [response] = result;

const ackIds = [];
for (const message of response.receivedMessages) {
  ...
  ackIds.push(message.ackId);
}

if (ackIds.length !== 0) {
  const ackRequest = {
    subscription: formattedSubscription,
    ackIds: ackIds,
  };

  await subClient.acknowledge(ackRequest);
}
...

```
