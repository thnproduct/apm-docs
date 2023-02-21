# Chaos Injection in Go SDK

### What Is a Span Listener?

Span listeners are the classes that inherit from the ThundraSpanListener abstract base class, and that have start and finish methods. When you register a span listener to Thundra, Thundra calls this span listener’s start and finish methods. This enables you to perform custom operations using a span listener during the life cycle of a span.

Thundra provides the following span listener implementations in our Go agent:

* ErrorInjectorSpanListener
* FilteringSpanListener
* LatencyInjectorSpanListener

### ErrorInjectorSpanListener

Parameters include:

* **errorMessage**: The message that displays the error description when you use ErrorInjectorSpanListener. The default value is “`Error injected by Thundra!`”.
* **injectOnFinish**: The Boolean value that decides whether the error will be injected after the span is finished or before it starts.
* **injectCountFreq**: The frequency of error injection; for example, if this value is `10`, then every tenth span event will be an error. The default value is `1`.
* **addInfoTags**: When this parameter is set to true, the span listener adds information tags to spans. These tags contain the span listener’s parameter names and values, allowing you to see this information on the corresponding span’s tags section in the Thundra console. The default value is `true`.

{% code title="Programmatic configuration of an ErrorInjectorSpanListener" %}
```go
config := map[string]string{
	"errorMessage":    "This is an injected error!",
	"injectOnFinish":  "true",
	"injectCountFreq": "3",
	"addInfoTags":     "true",
}

esl := tracer.NewErrorInjectorSpanListener(config).(*tracer.ErrorInjectorSpanListener)
tracer.RegisterSpanListener(esl)
```
{% endcode %}

### LatencyInjectorSpanListener

Parameters include:

* **delay**: The integer value that decides the latency in milliseconds that will be injected to spans. The default value is `0`.
* **injectOnFinish**: The Boolean value that decides whether the latency will be injected after the span is finished or before it starts.
* **randomizeDelay**: The Boolean value that decides the generating random delay value (if applicable). The default value is `false`.
* **addInfoTags**: When this parameter is set to true, the span listener adds information tags to spans. These tags contain the span listener’s parameter names and values, allowing you to see this information on the corresponding span’s tags section in the Thundra console. The default value is `true`.

{% code title="Programmatic configuration of a LatencyInjectorSpanListener" %}
```go
config := map[string]string{
	"errorMessage":    "This is an injected error!",
	"injectOnFinish":  "true",
	"injectCountFreq": "3",
	"addInfoTags":     "true",
}

esl := tracer.NewErrorInjectorSpanListener(config).(*tracer.ErrorInjectorSpanListener)
tracer.RegisterSpanListener(esl)
```
{% endcode %}

### FilteringSpanListener

Parameters include:

* **listener**: Takes span listener information to pass configuration. The listener can be `ErrorInjectorSpanListener`, `LatencyInjectorSpanListener`, or `FilteringSpanListener`.
* **config**: The configuration map holds common span listener configurations. By using config `errorMessage`, `injectOnFinish`, `injectCountFreq`, and `addInfoTags`, information can be added into the `listener`.
* **filter**: Span filters that decide whether or not to accept the given span.
* **domainName**: The domain name value to compare with the given span’s domain name.
* **className**: The class name value to compare with the given span’s class name.
* **operationName**: The operation name value to compare with the given span’s operation name.
* **tag**: Key-value tag pairs. `FilteringSpanListener` checks to see if each of the key-value pairs in this dictionary also exist in the given span’s tags. If at least one of the key-value pairs does not exist in the span’s tags or exists with a different value, then the `FilteringSpanListener` will reject the span.

{% code title="Programmatic configuration of a FilteringSpanListener" %}
```go
config := map[string]string{
    "listener":               "ErrorInjectorSpanListener",
    "config.errorMessage":    "You have a very funny name!",
    "config.injectOnFinish":  "true",
    "config.injectCountFreq": "1",
    "filter1.className":      "AWS-DynamoDB",
    "filter1.domainName":     "DB",
    "filter2.className":      "HTTP",
    "filter2.tag.http.host":  "google.com",
}

fsl := tracer.NewFilteringSpanListener(config).(*tracer.FilteringSpanListener)
tracer.RegisterSpanListener(fsl)
```
{% endcode %}
