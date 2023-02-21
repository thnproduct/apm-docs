# Chaos Injection in Python SDK

## Span Listeners

Span listeners are the classes that inherit from the `ThundraSpanListener` abstract base class, and that have `on_span_started(span)` and `on_span_finished(span)` methods. When you register a span listener to Thundra, Thundra calls this span listener’s `on_span_started(span)` and `on_span_finished(span)` methods whenever a span is started and finished. This enables you to perform custom operations using a span listener during the life cycle of a span.

Thundra provides the following span listener implementations:

* ErrorInjectorSpanListener
* LatencyInjectorSpanListener
* FilteringSpanListener

### ErrorInjectorSpanListener

Parameters include:

* **error\_message**: The message that displays the error description when using the ErrorInjectorSpanListener. The default value is `“Error injected by Thundra!”`.
* **error\_type**: The type of the error that will be raised. Use classes that inherit from the BaseException class for this parameter. Do not pass objects, but the class itself (e.g., pass NameError instead of NameError( ) ). The default value is Exception.
* **inject\_on\_finish**: The Boolean value that decides whether the error will be injected after the span is finished or before starting. The default value is `False`.
* **add\_info\_tags**: The span listener adds information tags containing the span listener’s parameter names and values to spans, but only when the add\_info\_tags parameter is set to True. This enables you to see the span listener parameters on the corresponding span’s tags section in the Thundra console. The default value is `True`.

{% code title="Programmatic configuration of an ErrorInjectorSpanListener" %}
```python
import redis
from thundra.plugins.trace import trace_support
from thundra.listeners import ErrorInjectorSpanListener
# Create the listener
error_sl = ErrorInjectorSpanListener(
    error_type=redis.ConnectionError,
    error_message="This is an injected error!"
    inject_on_finish=True
)
# Register the listener to use it
trace_support.register_span_listener(error_sl)
```
{% endcode %}

### LatencyInjectorSpanListener

Parameters include:

* **delay**: The integer value that decides the latency that will be injected to the spans in milliseconds. The default value is `0`.
* **variation**: When the `distribution` parameter is set to `uniform`, the `variation` parameter decides the range from which the injected latency value will be sampled. For example, if the `delay` is 500 and the `variation` is 150, then the amount of latency that will be injected to the span will be a uniform random variable between the values `[500-150, 500+150] = [350, 650]`. The default value is `0`.
* **sigma**: When the `distribution` parameter is set to `normal`, the `sigma` (standard deviation) parameter decides the range from which the injected latency value will be sampled. If the distribution parameter is `normal`, the amount of latency that will be injected to the span will be a normally distributed random variable with a standard deviation equal to the `sigma` parameter and a mean value equal to the `delay` parameter. The default value is `0`.
* **distribution**: The `distribution` parameter decides which distribution to use when sampling the amount of latency injected. If it is set to `uniform` value, then the span listener will use the `variation` and `delay` parameters to decide the amount of the latency. If it is set to `normal`, then the span listener will use the `sigma` parameter and the `delay` parameter to decide the amount of the latency. If it is set to a value other than `uniform` and `normal`, the span listener will behave like it is set to `uniform` and decide the amount of latency accordingly. The default value is `uniform`.

{% code title="Programmatic configuration of a LatencyInjectorSpanListener" %}
```python
from thundra.plugins.trace import trace_support
from thundra.listeners import LatencyInjectorSpanListener
# Create the listener
latency_sl = LatencyInjectorSpanListener(
    delay=2000,
    sigma=1000,
    distribution='normal',
)
# Register it to use it
trace_support.register_span_listener(latency_sl)
```
{% endcode %}

### FilteringSpanListener

Parameters include:

* **listener**: If the span is accepted after being filtered by `FilteringSpanListener`, then it is sent to the span listener given by the `listener` parameter. The default value is `None`.
* **filterer**: The filterer is the main object that decides whether or not the given span will be delegated to the `listener` parameter, meaning it is basically an object that has an accept method. Before delegating the given span to the `listener` parameter, `FilteringSpanListener` first passes the given span to the `filterer` object if the filterer’s accept method returns `True`. If it does, then the given span is passed to the `listener` parameter; if it does not, the given span is not passed to the listener. The default value is `None`.

{% code title="Programmatic configuration of a FilteringSpanListener" %}
```python
from thundra.listeners import FilteringSpanListener
from thundra.listeners import LatencyInjectorSpanListener
from thundra.plugins.trace import trace_support
from thundra.listeners.thundra_span_filterer import ThundraSpanFilterer,ThundraSpanFilter
# Create main listener
latency_sl = LatencyInjectorSpanListener(
    delay=2000,
    sigma=1000,
    distribution='normal',
)
# Create a filter
f1 = ThundraSpanFilter(
    class_name='AWS-Lambda',
    tags={'aws.lambda.name': 'upstream-lambda'}
)
# Create a filterer that uses a list of filters
filterer = ThundraSpanFilterer(span_filters=[f1])
# Create filtering span listener that takes the listener and the filterer
filtering_sl = FilteringSpanListener(
    listener=latency_sl,
    filterer=filterer,
)
# Register the listener to use it
trace_support.register_span_listener(filtering_sl)
```
{% endcode %}

**Configuration using environment variables:**\
In order to create a filtering span listener using an environment variable, you should set the `thundra_agent_lambda_trace_span_listener` environment variable. The basic structure is similar to the following:

{% code title="Basic structrure of a filtering span listener created using the environment variable" %}
```bash
thundra_agent_lambda_trace_span_listener: FilteringSpanListener[arguments]
```
{% endcode %}

Here is an example configuration that creates the same filtering span listener that we create programmatically above:

{% code title="A FilteringSpanListener configured using the environment variable" %}
```bash
thundra_agent_lambda_trace_span_listener: FilteringSpanListener[listener=LatencyInjectorSpanListener,config.delay=2000,config.sigma=1000,config.distribution=normal,filter1.className=AWS-Lambda,filter1.tag.aws.lambda.name=upstream-lambda]
```
{% endcode %}

There are a couple of points to note here:

* Using the `listener=LatencyInjectorSpanListener`, we state the type of listener that `FilteringSpanListener` is going to use. You can also use `ErrorInjectorSpanListener` when you need to inject an error.
* Each argument starting with the `config`. prefix is going to be passed to the `listener` parameter (which in this case is `LatencyInjectorSpanListener`) as parameters. It has a structure like this: `config.parameterName`. Note that we are using camel case whenever we need it. For example, if you used `ErrorInjectorSpanListener` as the listener argument, then you would pass the `error_message` parameter like this: `config.errorMessage=”Hello world!”`
* Each argument that starts with the `filter` keyword creates a filter. Any other character that follows the `filter` keyword is basically the id of that filter.
* For example, the two following arguments `filter1.className=AWS-Lambda`, `filter1.tag.aws.lambda.name=upstream-lambda` correspond to the following filter:

{% code title="Corresponding filter1 created using the environment variable" %}
```bash
filter1 = {
    class_name="AWS-Lambda", 
    tags={
        'aws.lambda.name': 'upstream-lambda'
    }
}
```
{% endcode %}

This results in a filtering span listener that would first filter the spans by looking at the `class_name` and the `tags` of the spans. If the given span has a different `class_name` than `“AWS-Lambda”` or the given span does not have the tag `aws.lambda.name` with the value `“upstream-lambda”`, then the span will be rejected and won’t be sent to the `listener`. If it does have both properties, then the filterer accepts the span and `FilteringSpanListener` propagates the span to its `listener` parameter (which in this case is a `LatencyInjectorSpanListener`).

### ThundraSpanFilterer

Parameters include:

* **span\_filters**: A list of span filters. Whenever a `FilteringSpanListener` calls the `ThundraSpanFilterer`’s accept method to decide whether or not to accept the given span, `ThundraSpanFilterer` internally calls the `accept(span)` method of each of the filters in the `span_filters`. If at least one of the filters in the `span_filters` accepts the span, or if the `span_filters` list is empty, then the given span is accepted and returned `True`. Otherwise, `ThundraSpanFilterer`’s accept method returns `False`. The default value is `[]`.

### ThundraSpanFilter

Parameters include:

* **domain\_name**: The domain name value to compare with the given span’s domain name. The default value is `None`.
* **class\_name**: The class name value to compare with the given span’s class name. The default value is `None`.
* **operation\_name**: The operation name value to compare with the given span’s operation name. The default value is `None`.
* **tags**: A dictionary that contains key-value tag pairs. `ThundraSpanFilter` checks to ensure that each of the key-value pairs in this dictionary also exists in the given span’s tags. If at least one of the key-value pairs does not exist in the span’s tags or exists with a different value, then the `ThundraSpanFilter` rejects the span. The default value is `None`.

