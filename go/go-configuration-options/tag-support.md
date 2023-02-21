# Tagging in Go SDK

With Thundra's Go agent, you can add custom tags to your invocations or your applications, which enables you to better filter data. Invocation tags are added only to the `Invocation` data type; application tags are global and are automatically added to `Invocation`, `Metric`, `Trace`, `Span`, and `Log` data.

### Adding Invocation Tags

The following code snippet shows how to add tags to an invocation:

{% code title="Go" %}
```go
package main

import (
	"context"

	"github.com/thundra-io/thundra-lambda-agent-go/invocation"

	"github.com/aws/aws-lambda-go/lambda"
	"github.com/thundra-io/thundra-lambda-agent-go/thundra"
)

// Your main lambda handler
func handler(ctx context.Context, userID string) (string, error) {
	invocation.SetTag("userID", userID)
	invocation.SetError(errors.New("custom error"))

	return "Hello ƛ!", nil
}

func main() {
	// Wrap your lambda handler with Thundra
	lambda.Start(thundra.Wrap(handler))
}
```
{% endcode %}

### Adding Application Tags

You can add application tags using environment variables. All application tags that start with `thundra_agent_lambda_application_tag_` are parsed by Thundra's Go agent. The text after the `thundra_agent_lambda_application_tag_` prefix is used as the key of the tag and the value of the environment variable becomes the value of the tag. For example, `thundra_agent_lambda_application_tag_floatField` with the value `12.123` will add an application tag to all data with the key `floatField` and value `12.123`.

The example below shows how to add application tags with various data types to a Lambda invocation.

![](../../.gitbook/assets/go\_tag\_application.png)
