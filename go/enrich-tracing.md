# Enrich Tracing in Go

### Manual Instrumentation with Open Tracing

Thundra uses [OpenTracing API](https://github.com/opentracing/opentracing-go) to implement instrumentation. This allows you to manually instrument your code by following [OpenTracing API instructions](https://github.com/opentracing/opentracing-go) as Thundra’s agents are compliant with OpenTracing API.

To manually instrument your Go functions and see detailed spans, you will have to create `span` instances using the OpenTracing's `StartSpan` methods. OpenTracing API provides two different methods that you can use to start new spans in your function: `opentracing.StartSpan` and `opentracing.StartSpanFromContext`. We suggest using the `opentracing.StartSpanFromContext` method to create new spans by passing it a context, an operation name, and optional parameters. Refer to our [section about using contexts](go-configuration-options/trace-configurations.md#using-contexts-while-creating-new-spans) to learn more.

{% hint style="info" %}
#### You Don't Need to Create a Span for the Main Handler Function

Thundra automatically creates a root span representing the main Lambda handler function and finishes this root span when your Lambda function is done. Thus, you don't need to create a root span representing your handler function. Any other span you create in your main handler or in other functions will be the child of this root span.
{% endhint %}

The following code snippet shows an example usage of OpenTracing API to create new spans:

{% code title="Creating a new span" %}
```go
// Pass a context parameter to the function
func sayHello(ctx context.Context, name string) {
	// Create a new span to represent operations made in this function
	span, _ := opentracing.StartSpanFromContext(ctx, "sayHello")
	// Finish the span when function is done
	defer span.Finish()
	// Perform actual function logic
	fmt.Printf("Hello, %s!\n", name)
}
```
{% endcode %}

### Using Contexts While Creating New Spans

Currently, Golang does not provide a local storage or a similar construct for threads. For this reason, OpenTracing API handles parent-child relations between spans using Golang's context objects.

Since Thundra creates a root span representing your main handler function, we strongly suggest you create your main handler function so that it accepts a context object as its first argument. This way, you will be able to access the root span that Thundra has created to represent your main handler function and use the passed context variable to create child spans from this root span.

The following code snippet shows how context objects can be used to create child spans:

{% code title="Go" %}
```go
package main

import (
	"context"
	"time"

	"github.com/aws/aws-lambda-go/lambda"
	opentracing "github.com/opentracing/opentracing-go"
	"github.com/thundra-io/thundra-lambda-agent-go/thundra"
)

// Your main lambda handler
func handler(ctx context.Context) (string, error) {
	// Currently ctx object contains the root span that
	// Thundra have created for you

	// Sleep some amount, representing the operations
	// that you make in your handler function before
	// creating a new child span
	time.Sleep(time.Millisecond * 100)

	// Say you continue to make some operations in your
	// main handler but this time create a childSpan
	// to represent these operations.
	aSpan, _ := opentracing.StartSpanFromContext(ctx, "childSpan-1")
	time.Sleep(time.Millisecond * 100)
	aSpan.Finish()

	// Make a call to another function. Note that this function
	// also accepts a context object. We are using this context
	// object to carry the parent span information.
	aFunction(ctx)

	// Creating another span representing the another operations
	// that we are doing inside the root span.
	anotherSpan, _ := opentracing.StartSpanFromContext(ctx, "childSpan-3")
	time.Sleep(time.Millisecond * 100)
	anotherSpan.Finish()

	// Say we have also some operations before our handler finishes
	time.Sleep(time.Millisecond * 50)

	return "Hello ƛ!", nil
}

func aFunction(ctx context.Context) {
	span, _ := opentracing.StartSpanFromContext(ctx, "childSpan-2")
	defer span.Finish()

	time.Sleep(time.Millisecond * 100)
}

func main() {
	// Wrap your lambda handler with Thundra
	lambda.Start(thundra.Wrap(handler))
}
```
{% endcode %}

The resulting trace chart that you will see in the Thundra web console for this function will be similar to the following image.

![](../.gitbook/assets/go\_manual\_trace.png)

As you can see, there is the root span that Thundra automatically created while other spans are the child of the root span, as they should be.
