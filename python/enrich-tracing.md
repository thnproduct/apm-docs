# Enrich Tracing in Python

### Manual Instrumentation with Open Tracing

Configuring Thundra’s Trace Support in the Python agent involves declaring a trace object at the start of your Lambda function. Trace Support is enabled by default  when you wrap your function; however, if you would like to configure Trace Support and also perform manual instrumentation, then you must get a tracer instance with the command `ThundraTracer.get_instance()`

#### Tracing Code Blocks Using Context Manager

To trace a code block, you can use the context manager returned calling

```python
tracer.start_active_span(operation_name="write_user_to_db")
```

where `operation_name` indicates the name of the span relevant to the traced block of code.

```python
from thundra.opentracing.tracer import ThundraTracer

def save_user(name, surname, id):
    # Get tracer instance
    tracer = ThundraTracer.get_instance()
    # Trace code block with context manager
    with tracer.start_active_span(operation_name="write_user_to_db") as scope:
        # Perform operations to be traced
        # ...
        
```

If you already use OpenTracing to instrument your code, you can overwrite the `opentracing.tracer` with a Thundra tracer instance:

```python
# Get tracer instance
tracer = ThundraTracer.get_instance()
# Set the tracer
opentracing.tracer = tracer
```

An example usage is show below:

```python
from thundra.opentracing.tracer import ThundraTracer

def save_user(name, surname, id):
    # Get tracer instance
    tracer = ThundraTracer.get_instance()
    opentracing.tracer = tracer
    
    span = tracer.start_span(operation_name="operation_name")
    span.set_tag('key', 'value')
    # Do some operations ...
    # ...
    span.finish()
```

#### Tracing Methods Using @Traceable Decorator

To trace a method using automated instrumentation, import `Traceable` from Thundra’s trace plugin, as shown below:

```python
from thundra.plugins.trace.traceable import Traceable
```

Then, you can use the `@Traceable` decorator to trace your functions:

```python
@Traceable(trace_args=True, trace_return_value=True, trace_error=True, trace_err)
def method_to_be_traced():
  """ Traced method """
  # ...
```

**Offline Debugging**

Offline debugging tracing is supported by Thundra's Python agent, and it allows you to monitor your Lambda functions by each line of code. By default, offline debugging is disabled, but can be enabled by passing arguments to the `@Traceable` decorator.

Offline debugging can be enabled by setting `trace_line_by_line` to `True`.

```python
@Traceable(trace_line_by_line=True)
def method_to_be_traced():
  """ Traced method """
  # ...
```

Local variables in the method can be traced by setting `trace_local_variables` to `True` when offline debugging is enabled.

```python
@Traceable(trace_line_by_line=True, trace_local_variables=True)
def method_to_be_traced():
  """ Traced method """
  # ...
```

Source code in lines can be traced by setting `trace_lines_with_source` to `True` when offline debugging is enabled.

```python
@Traceable(trace_line_by_line=True, trace_lines_with_source=True)
def method_to_be_traced():
  """ Traced method """
  # ...
```

**Configuring Trace Arguments**

By configuring trace arguments, you can keep track of the arguments within the scope of your span. Trace arguments are disabled by default; however, they can easily be enabled by adding `trace_args = True` in the `@Traceable`decorator.

**Configuring Trace Return Values**

By configuring trace return values, you can monitor all values within the scope of your span. Trace return values are disabled by default; however, they can easily be enabled by adding `trace_return_value = True` in the `@Traceable` decorator.

**Configuring Trace Errors**

By configuring trace errors, you can monitor any errors that are created under the scope of your span. Trace errors are disabled by default; however, they can easily be enabled by adding `trace_error = True` in the `@Traceable` decorator.&#x20;



