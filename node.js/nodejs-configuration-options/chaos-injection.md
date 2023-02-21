# Chaos Injection in Node.js SDK

## Span Listeners

Span listeners are the classes that implement the `ThundraSpanListener` interface, and that have `onSpanStarted` and `onSpanFinished` methods. When you register a span listener to Thundra, Thundra calls this span listener’s `onSpanStarted` and `onSpanFinished` methods whenever a span is started and finished. This enables you to perform custom operations using a span listener during the life cycle of a span.

There are three span listeners implemented in the Thundra Node.js agent:

* ErrorInjectorSpanListener
* FilteringSpanListener
* LatencyInjectorSpanListener

### ErrorInjectorSpanListener

Parameters include:

* **errorMessage**: The message that displays the error description when using the ErrorInjectorSpanListener. The default value is `“Error injected by Thundra!”`.
* **errorType**: The type of error that will be raised. The name of the thrown error will be set to this value.
* **injectOnFinish**: The Boolean value that decides whether the error will be injected after the span is finished or before starting. The default value is `False`.
* **injectCountFreq:** The frequency for error injection. For example, if this value is 10, then every 10th span event will be an error. The default value is 1.

{% code title="Programmatic configuration of an ErrorInjectorSpanListener" %}
```javascript
const thundra = require("@thundra/core");

const thundraWrapper = thundra({
    apiKey: config.thundra.api_key
});

// Import Listener;
const ErrorInjectorSpanListener = thundra.listeners.ErrorInjectorSpanListener;

// Configure Listener;
const errorInjectorListenerConfig = {
    injectCountFreq:1,
    errorType: 'RedisError',
    errorMessage: 'Kaboom!!! Redis crashed.',
    injectOnFinish: true
};

// Create Listener;
const errorInjectorListener = new ErrorInjectorSpanListener(errorInjectorListenerConfig);

// Add Listener;
thundra.tracer().addSpanListener(errorInjectorListener);

exports.handler = thundraWrapper((event, context, callback) => {
  // Your Lambda Code
});
```
{% endcode %}

### LatencyInjectorSpanListener

Parameters include:

* **delay**: The integer value that decides the latency that will be injected to spans in milliseconds. The default value is `100`.

{% code title="Programmatic configuration of a LatencyInjectorSpanListener" %}
```javascript
const thundra = require("@thundra/core");

const thundraWrapper = thundra({
    apiKey: config.thundra.api_key
});

// Import Listener;
const LatencyInjectorSpanListener = thundra.listeners.LatencyInjectorSpanListener;

// Configure Listener;
const latencyInjectorSpanListenerConfig = {
    delay: 5000
};

// Create Listener;
const latencyInjectorSpanListener = new LatencyInjectorSpanListener(latencyInjectorSpanListenerConfig);

// Add Listener;
thundra.tracer().addSpanListener(latencyInjectorSpanListener);

exports.handler = thundraWrapper((event, context, callback) => {
  // Your Lambda Code
});
```
{% endcode %}

### FilteringSpanListener

Parameters include:

* **listener**: If the span is accepted after being filtered by `FilteringSpanListener`, then it is sent to the span listener given by the listener parameter. The default value is `None`.
* **spanFilterer**: Filterer is the main parameter that decides whether or not the given span will be delegated to the `listener` parameter, meaning it is basically an object that has an accept method. Before delegating the given span to the `listener` parameter, `FilteringSpanListener` first passes the given span to the `filterer` object if the filterer’s accept method returns True. If it does, the given span is passed to the `listener` parameter; if it does not, the given span is not passed to the listener. The default value is `None`.

{% code title="Programmatic configuration of a FilteringSpanListener" %}
```javascript
const thundra = require("@thundra/core");

const thundraWrapper = thundra({
    apiKey: config.thundra.api_key
});

//Import Statements;
const FilteringSpanListener = thundra.listeners.FilteringSpanListener;
const ErrorInjectorSpanListener = thundra.listeners.ErrorInjectorSpanListener;
const StandardSpanFilterer = thundra.listeners.StandardSpanFilterer;
const SpanFilter = thundra.listeners.SpanFilter;


// Create a span filter for Redis Get Commands
const filter = new SpanFilter();
filter.className = 'Redis';
filter.tags = {
    'redis.command' : 'GET'
};

// Create a StandardSpanFilterer with one filter
const filterer = new StandardSpanFilterer([filter]);

const errorInjectorListenerConfig = {
    injectCountFreq:1 ,
    errorType: 'RedisError',
    errorMessage: 'Kaboom!!! Redis crashed.',
    injectOnFinish: true
};

// Create a ErrorInjectorSpanListener with one filter
const errorInjectorListener = new ErrorInjectorSpanListener(errorInjectorListenerConfig);

// Create a FilteringSpanListener with spanFilterer and listener
const filteringListener = new FilteringSpanListener();
filteringListener.listener = errorInjectorListener;
filteringListener.spanFilterer = filterer;

thundra.tracer().addSpanListener(filteringListener);

exports.handler = thundraWrapper((event, context, callback) => {
  // Your Lambda Code
});
```
{% endcode %}

**Configuration using environment variables:**\
In order to create a filtering span listener using an environment variable, you should set the `thundra_agent_lambda_trace_span_listener` environment variable. The basic structure is similar to the following:

{% code title="Basic structrure of a filtering span listener created using the environment variable" %}
```yaml
thundra_agent_lambda_trace_span_listener: FilteringSpanListener[arguments]
```
{% endcode %}

Here is an example configuration that creates the same filtering span listener that we create programmatically above:

{% code title="A FilteringSpanListener configured using the environment variable" %}
```yaml
thundra_agent_lambda_trace_span_listener: FilteringSpanListener[listener=LatencyInjectorSpanListener,config.delay=2000,filter1.className=AWS-Lambda,filter1.tag.aws.lambda.name=upstream-lambda]
```
{% endcode %}

There are a couple of points to note here:

* Using the `listener=LatencyInjectorSpanListener`, we state the type of the listener that `FilteringSpanListener` is going to use. You can also use `ErrorInjectorSpanListener` when you need to inject an error.
* Each argument starting with the `config.` prefix is going to be passed to the listener parameter (which in this case is `LatencyInjectorSpanListener`) as parameters. It has a structure like this: `config.parameterName`. Note that we are using camel case whenever we need it. For example, if you used `ErrorInjectorSpanListener` as the listener argument, then you would pass the `error_message` parameter like this: `config.errorMessage=”Hello world!”`
* Each argument that starts with the filter keyword creates a filter. Any other character that follows the filter keyword is basically the id of that filter.
* For example, the two following arguments `filter1.className=AWS-Lambda`, `filter1.tag.aws.lambda.name=upstream-lambda` correspond to the following filter:

{% code title="Corresponding filter1 created using the environment variable" %}
```javascript
filter1 = {
    class_name="AWS-Lambda", 
    tags={
        'aws.lambda.name': 'upstream-lambda'
    }
}
```
{% endcode %}

This results in a filtering span listener that would first filter the spans by looking at the `class_name` and the `tags` of the spans. If the given span has a different `class_name` than “`AWS-Lambda`” or the given span does not have the tag `aws.lambda.name` with the value “`upstream-lambda`”, then the span will be rejected and won’t be sent to the `listener`. If it does have both properties, then the `filterer` accepts the span and `FilteringSpanListener` propagates the span to its `listener` parameter (which in this case is a `LatencyInjectorSpanListener`).

### ThundraSpanFilterer

Parameters include:

* **spanFilters**: A list of span filters. Whenever a `FilteringSpanListener` calls the `ThundraSpanFilterer`’s accept method to decide whether or not to accept the given span, `ThundraSpanFilterer` internally calls the `accept(span)` method of each of the filters in the `spanFilters`. If at least one of the filters in the `spanFilters` accepts the span, or if the `spanFilters` list is empty, then the given span is accepted and returned True. Otherwise, `ThundraSpanFilterer`’s accept method returns `False`. The default value is `[]`.

### ThundraSpanFilter

Parameters include:

* **domainName**: The domain name value to compare with the given span’s domain name. The default value is `None`.
* **className**: The class name value to compare with the given span’s class name. The default value is `None`.
* **operationName**: The operation name value to compare with the given span’s operation name. The default value is `None`.
* **tags**: A dictionary that contains key-value tag pairs. `ThundraSpanFilter` checks to ensure that each of the key-value pairs in this dictionary also exists in the given span’s tags. If at least one of the key-value pairs is not in the span’s tags or exists with a different value, then the `ThundraSpanFilter` rejects the span. The default value is `None`.
