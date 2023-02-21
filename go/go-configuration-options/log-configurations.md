# Configuring Logs in Go SDK

{% hint style="info" %}
The Thundra Go agent's log plugin is disabled by default. In addition, the Thundra console now gets log data through CloudWatch integration. Older agent versions can continue to send log data, but for optimal performance, it is recommended to use the latest version and add the integration.&#x20;
{% endhint %}

### Configuring Go Log Support

Thundra provides a logger that you can access through thundra.Logger. The logs that you create using this logger will be available in the Thundra web console under the corresponding invocation.

`thundra.Logger` is just a wrapper on the Go's log package, which is why the methods of `thundra.Logger`are very similar to log package methods.

Log methods that you can use with `thundra.Logger`:

* `Trace(v ...interface{})`
* `TraceWithSpan(span opentracing.Span, v ...interface{})`
* `Debug(v ...interface{})`
* `DebugWithSpan(span opentracing.Span, v ...interface{})`
* `Info(v ...interface{})`
* `InfoWithSpan(span opentracing.Span, v ...interface{})`
* `Warn(v ...interface{})`
* `WarnWithSpan(span opentracing.Span, v ...interface{})`
* `Error(v ...interface{})`
* `ErrorWithSpan(span opentracing.Span, v ...interface{})`
* `Printf(format string, v ...interface{})`
* `Print(v ...interface{})`
* `Println(v ...interface{})`
* `Panic(v ...interface{})`
* `Panicln(v ...interface{})`
* `Panicf(format string, v ...interface{})`

These log methods work in the same way as Go's log package methods, the only difference being that we have some extra methods with the name pattern "\*WithSpan" which accept an extra span parameter. If you log using these methods, you will be able to see your log under the corresponding span in the Thundra web console. If you don’t use these methods, your logs will be available under the Invocation Logs tab and will be bound to a specific span rather than the invocation itself.

The following code snippet shows usage of `thundra.Log`:

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
	// Log without span
	thundra.Logger.Info("This is an info log")
	// Call another function
	firstFunction(ctx)

	return "Hello ƛ!", nil
}

func firstFunction(ctx context.Context) {
	span, _ := opentracing.StartSpanFromContext(ctx, "childSpan")
	defer span.Finish()
	// Log using a span
	thundra.Logger.WarnWithSpan(span, "This a warning log")
	time.Sleep(time.Millisecond * 100)
}

func main() {
	// Wrap your lambda handler with Thundra
	lambda.Start(thundra.Wrap(handler))
}

```
{% endcode %}

As previously stated, no matter what log method you use, all the logs you have created using `thundra.Logger` will be available under the Invocation Log tab.

### Configuring Log Plugin

You can configure the log plugin using the following environment variables:

`thundra_agent_lambda_log_disable`

Since the log plugin is **disabled** by default, you will need to set this variable to false if you want to enable the plugin.

![](../../.gitbook/assets/go\_log.png)

`thundra_log_logLevel`

Set this variable to the log level you want to use. By default, all the logs are reported.

![](<../../.gitbook/assets/go\_log\_span (1).png>)

\
