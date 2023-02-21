# Integration Options for Go SDK

{% tabs %}
{% tab title="Programmatic Configuration" %}
#### Step 1: Installation

Install the agent using the following command:

{% code title="Import thundra-lambda-agent-go" %}
```go
import "github.com/thundra-io/thundra-lambda-agent-go/v2/thundra"	      // with go modules enabled (GO111MODULE=on or outside GOPATH) for version >= v2.3.1
import "github.com/thundra-io/thundra-lambda-agent-go/thundra"         // with go modules disabled
```
{% endcode %}

#### Step 2: Wrap Your Handler

Next, wrap your Lambda handler with `thundra.wrap`:

{% code title="Wrap your lambda handler" %}
```go

package main

import (
	"github.com/aws/aws-lambda-go/lambda"
  // thundra go agent import here
)

// Your lambda handler
func handler() (string, error) {
	return "Hello, Thundra!", nil
}

func main() {
	// Wrap your lambda handler with Thundra
	lambda.Start(thundra.Wrap(handler))
}
```
{% endcode %}

#### Step 3: Build and Deploy

Build and deploy your executable to AWS as you usually do. In order to see your invocations in the Thundra web console, make sure that you have set the `thundra_apiKey` environment variable to the API key you received from the Thundra web console. You can set the environment variables through the AWS Lambda console, in your `serverless.yml` file, or using another method of your choice.

{% code title="The API Key environment variable" %}
```
thundra_apiKey: your_api_key
```
{% endcode %}

#### Step 4: Invoke Your Function

Now you can invoke your Lambda function and see the details of your invocation in the Thundra console!
{% endtab %}
{% endtabs %}

###
