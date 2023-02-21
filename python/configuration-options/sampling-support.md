# Sampling in Python SDK

You can reduce the amount of data that the Python agent sends to the Thundra Web Console by enabling sampling. You can either add built-in sampling rules or you can provide a custom sampler and implement the sampling logic by yourself. Either option allows you to sample **Trace**, **Metric**, and/or **Log** data using the Sampling API.

## Sampling API

A sampler should implement the abstract class shown below:

```python
from abc import ABC, abstractmethod

class BaseSampler(ABC):

    @abstractmethod
    def is_sampled(self, args):
        raise Exception("should be implemented")
```

### Custom Sampler

A custom sampler is a class with the `is_sampled` function. The `is_sampled` function should return as `true` if you want to send data to Thundra, and should return as `false` if you want to omit the data.

```python
from thundra.samplers import BaseSampler

class CustomSampler(BaseSampler):
  def is_sampled(self, data=None):
    # TODO: Your custom logic here
```

### Count Aware Sampler

The Count Aware Sampler enables you to sample metric data with a count frequency. For example, if the count frequency is 500, data will be sampled with every 500 Lambda invocations.

```python
from thundra.samplers import CountAwareSampler

# Sample for every 500 call
sampler = CountAwareSampler(count_freq=500)
```

You can update count frequency without re-deploying Lambda functions. After setting Count Aware Sampler as the sampler, you can change the count frequency by setting the `thundra_agent_lambda_metric_sample_sampler_countAware_countFreq` environment variable in AWS Lambda.

### Time Aware Sampler

The Time Aware Sampler enables you to sample metric data with a time frequency. For example, if the time frequency is 300,000, data will be sampled every 300 milliseconds within consecutive Lambda invocations.

```python
from thundra.samplers import TimeAwareSampler

# Sample for every 1000 milliseconds
sampler = TimeAwareSampler(time_freq=1000)
```

You can update time frequency without re-deploying Lambda functions. After setting Time Aware Sampler as the sampler, you can change the time frequency by setting the `thundra_agent_lambda_metric_sample_sampler_timeAware_timeFreq` environment variable in AWS Lambda.

### Composite Sampler

The Composite Sampler enables you to combine multiple `Sampler` using the specified composition operation (`and` or `or`).

An example using `TimeAwareSampler` and `CountAwareSampler` with or can be seen below:

```python
from thundra.samplers import (
    TimeAwareSampler, CountAwareSampler,
    CompositeSampler
)


sampler = CompositeSampler(
    samplers=[
        TimeAwareSampler(),
        CountAwareSampler()
    ],
    operator='or'
)
```

## Metric Sampling

To use a sampler for metric sampling, you should set the sampler using the `set_sampler` function from `thundra.plugins.metric.metric_support`.

{% hint style="info" %}
#### Default Metric Sampling

By default, metrics are sampled every 100 invocation or every 5 minutes, whichever comes first. Default invocation count and time frequencies can be configured through the `thundra_agent_lambda_sampler_countAware_countFreq` and `thundra_agent_lambda_sampler_timeAware_timeFreq` environment variables. You can also set your own custom sampler to be the preferred method over default configuration.
{% endhint %}

The samplers available include:

* Composite Sampler
* Count Aware Sampler
* Time Aware Sampler

### Count Aware Sampler

To use the Count Aware Sampler for metric sampling, you should import `thundra.samplers.CountAwareSampler` and set it as the sampler using the `set_sampler` function from `thundra.plugins.metric.metric_support`. See the example below:

```python
from thundra.thundra_agent import Thundra
from thundra.samplers import CountAwareSampler
from thundra.plugins.metric import metric_support

metric_support.set_sampler(CountAwareSampler(count_freq=500))
thundra = Thundra()

# Main handler
@thundra
def handler(event, context):  
    return {"result": "hello"}
```

You can update count frequency without re-deploying Lambda functions. After setting Count Aware Sampler as the sampler, you can change the count frequency by setting the `thundra_agent_lambda_metric_sample_sampler_countAware_countFreq` environment variable in AWS Lambda.

### Time Aware Sampler

To use Time Aware Sampler for metric sampling, you should import `thundra.samplers.TimeAwareSampler` and set it as the sampler using the `set_sampler` function from `thundra.plugins.metric.metric_support`. See the example below:

```python
from thundra.thundra_agent import Thundra
from thundra.samplers import TimeAwareSampler
from thundra.plugins.metric import metric_support

metric_support.set_sampler(TimeAwareSampler(time_freq=1000))
thundra = Thundra()

# Main handler
@thundra
def handler(event, context):  
    return {"result": "hello"}
```

You can update time frequency without re-deploying Lambda functions. After setting Time Aware Sampler as the sampler, you can change the time frequency by setting the `thundra_agent_lambda_metric_sample_sampler_timeAware_timeFreq` environment variable in AWS Lambda.

### Custom sampler

An example usage of a custom sampler for the metric plugin is shown below:

```python
import requests

from thundra.thundra_agent import Thundra

from thundra.samplers import BaseSampler
from thundra.plugins.metric import metric_support

class CustomSampler(BaseSampler):
  def is_sampled(self, data=None):
    # TODO: Your custom logic here

metric_support.set_sampler(CustomSampler())

thundra = Thundra()
# Main handler
@thundra
def handler(event, context):
  return {"result": "hello"}
```

## Log Sampling

To use a sampler for log sampling, you should set the sampler using the `set_sampler` function from `thundra.plugins.log.log_support`.

The samplers available include:

* Composite Sampler
* Count Aware Sampler
* Time Aware Sampler

### Custom sampler

An example usage of a custom sampler for the log plugin is shown below:

```python
import requests

from thundra.thundra_agent import Thundra

from thundra.samplers import BaseSampler
from thundra.plugins.log import log_support

class CustomSampler(BaseSampler):
  def is_sampled(self, log=None):
    # TODO: Your custom logic here

log_support.set_sampler(CustomSampler())

thundra = Thundra()
# Main handler
@thundra
def handler(event, context):
  return {"result": "hello"}
```

## Trace Sampling

To use a sampler for trace sampling, you should set the sampler using the `set_sampler` function from `thundra.plugins.trace.trace_support`.

{% hint style="info" %}
#### Sampling Target

For traces, sampling is only applied to the root span (not the other internal spans) of the invocation which represents the actual Lambda invocation. According to decision, if the root span is sampled, then all of the spans in the invocation are sampled.
{% endhint %}

The samplers available include:

* Composite Sampler
* Duration Aware Sampler
* Error Aware Sampler

### Duration Aware Sampler

The Duration Aware Sampler enables you to sample trace data according to the duration of the Lambda. This sampler is ideal if, for example, you want to send trace data only if the duration of the Lambda is longer than 500 milliseconds.

See the example below:

{% code title="Duration Aware Sampler" %}
```python
import requests

from thundra.thundra_agent import Thundra
from thundra.plugins.log.thundra_log_handler import ThundraLogHandler

from thundra.samplers import DurationAwareSampler
from thundra.plugins.trace import trace_support

trace_support.set_sampler(DurationAwareSampler(500, True))

thundra = Thundra()
# Main handler
@thundra
def handler(event, context):  
    return {"result": "hello"}
```
{% endcode %}

### Error Aware Sampler

The Error Aware Sampler enables you to sample trace data according to erroneous invocations of a Lambda. For example, this would be ideal to use if you want to send trace data only from a Lambda whose execution results in an error.

See an example below:

{% code title="Error Aware Sampler" %}
```python
import requests

from thundra.thundra_agent import Thundra

from thundra.samplers import ErrorAwareSampler
from thundra.plugins.trace import trace_support

trace_support.set_sampler(ErrorAwareSampler())

thundra = Thundra()
# Main handler
@thundra
def handler(event, context):  
    return {"result": "hello"}
```
{% endcode %}

### Custom sampler

A custom sampler can be added for the trace plugin by setting the sampler using the `set_sampler` function from `thundra.plugins.trace.trace_support`.

{% code title="Custom Sampler" %}
```python
import requests

from thundra.thundra_agent import Thundra

from thundra.samplers import BaseSampler
from thundra.plugins.trace import trace_support

class CustomSampler(BaseSampler):
  def is_sampled(self, span=None):
    # TODO: Your custom logic here

trace_support.set_sampler(CustomSampler())

thundra = Thundra()
# Main handler
@thundra
def handler(event, context):
  return {"result": "hello"}
```
{% endcode %}

