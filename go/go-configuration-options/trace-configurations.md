# Configuring Traces for Go SDK

### Configuring the Trace Plugin

You can configure the trace plugin using the following environment variables.

Set the variable below to `true` if you want to disable the trace plugin; the trace plugin is enabled by default.

{% code title="Configuration of Trace Plugin" %}
```yaml
thundra_agent_lambda_trace_disable
```
{% endcode %}

Set the variable below to `true` if you want to disable monitoring of Lambda requests.

{% code title="Configuration of Trace Request" %}
```yaml
thundra_agent_lambda_trace_request_skip
```
{% endcode %}

Set the variable below to `true` if you want to disable monitoring Lambda responses.

{% code title="Configuration of Trace Response" %}
```yaml
thundra_agent_lambda_trace_response_skip
```
{% endcode %}

### Configuring AWS SDK Trace

Thundra's Go agent provides you with the capability to trace AWS SDK by wrapping the `session` object that AWS SDK provides. You can easily start using it by simply wrapping your session objects as shown in the following code:

{% code title="Go" %}
```go
package main

import (
	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/dynamodb"
	"github.com/thundra-io/thundra-lambda-agent-go/thundra"
	thundraaws "github.com/thundra-io/thundra-lambda-agent-go/wrappers/aws"
)

// Your lambda handler
func handler() (string, error) {
	// Create a new session object
	sess, _ := session.NewSession(&aws.Config{
		Region: aws.String("us-west-2")},
	)
	
	// Wrap it using the thundraaws.Wrap method
	sess = thundraaws.Wrap(sess)

	// Create a new client using the wrapped session
	svc := dynamodb.New(sess)

	// Use the client as normal, Thundra will automatically
	// create spans for the AWS SDK calls
	svc.PutItem(&dynamodb.PutItemInput{
		Item: map[string]*dynamodb.AttributeValue{
			"AlbumTitle": {
				S: aws.String("Somewhat Famous"),
			},
			"Artist": {
				S: aws.String("No One You Know"),
			},
			"SongTitle": {
				S: aws.String("Call Me Today"),
			},
		},
		ReturnConsumedCapacity: aws.String("TOTAL"),
		TableName:              aws.String("Music"),
	})

	return "Hello, Thundra!", nil
}

func main() {
	// Wrap your lambda handler with Thundra
	lambda.Start(thundra.Wrap(handler))
}
```
{% endcode %}

After you have wrapped the session object, everything else will be handled automatically. The spans representing AWS SDK calls will be shown in your trace chart.

#### Disabling AWS Integrations

Tracing AWS SDK responses is enabled by default, but this can also be disabled if needed through configuration. You just need to set the following environment variable to `true`:

{% code title="Configuration of Disabling Aws Integrations" %}
```yaml
thundra_agent_lambda_trace_integrations_aws_disable
```
{% endcode %}

#### Masking SQS Messages

You can mask SQS messages sent on the client’s side, which calls AWS SDK. You just need to set the following environment variable to `true`:

{% code title="Masking SQS Messages" %}
```yaml
thundra_agent_lambda_trace_integrations_aws_sqs_message_mask
```
{% endcode %}

#### Masking SNS Messages

You can mask SNS messages sent on the client’s side, which calls AWS SDK. You just need to set the following environment variable to `true`:

{% code title="Masking SNS Messages" %}
```
thundra_agent_lambda_trace_integrations_aws_sns_message_mask
```
{% endcode %}

#### Masking Lambda Payload

You can mask Lambda invocation payload on the client’s side, which calls AWS SDK. You just need to set the following environment variable to `true`.

{% code title="Masking Lambda Payload" %}
```yaml
thundra_agent_lambda_trace_integrations_aws_lambda_payload_mask
```
{% endcode %}

#### Masking HTTP Body

You can mask an HTTP body on the client’s side, which calls AWS SDK. You just need to set the following environment variable to `true`:

{% code title="Masking HTTP Body" %}
```
thundra_agent_lambda_trace_integrations_aws_http_body_mask
```
{% endcode %}

