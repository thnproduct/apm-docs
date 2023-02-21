# Sampling in Go SDK

You can reduce the amount of data that the Go agent sends to the Thundra web console by enabling sampling. You can either add built-in sampling rules to the `metric` and `trace` plugin or provide a custom sampler and implement the sampling logic by yourself. Do note, however, that you can only sample timed-out invocations.

### Count Aware Sampler

The Count Aware Sampler enables you to sample data with a count frequency. For example, if the count frequency is set to 50, metric data will be sampled after every 50 Lambda invocations. The configuration is shown below:

{% code title="Programmatic Configuration of CountAwareSampler" %}
```go
import(
    "github.com/thundra-io/thundra-lambda-agent-go/samplers"
    "github.com/thundra-io/thundra-lambda-agent-go/metric"
    "github.com/thundra-io/thundra-lambda-agent-go/log"
	"github.com/thundra-io/thundra-lambda-agent-go/trace"   
)

cas := NewCountAwareSampler(5) // Sample data every 5th invocation
log.SetSampler(cas) // Sample log data
metric.SetSampler(cas) // Sample metric data
trace.SetSampler(cas) // Sample trace data
```
{% endcode %}

### Time Aware Sampler

The Time Aware Sampler enables you to sample data with a time frequency. For example, if the time frequency is set to 300,000, metric data will be sampled every 300 seconds within consecutive Lambda invocations. The configuration is shown below:

{% code title="Programmatic Configuration of TimeAwareSampler" %}
```go
import(
    "github.com/thundra-io/thundra-lambda-agent-go/samplers"
    "github.com/thundra-io/thundra-lambda-agent-go/metric"
    "github.com/thundra-io/thundra-lambda-agent-go/log"
	"github.com/thundra-io/thundra-lambda-agent-go/trace"   
)

tas := samplers.NewTimeAwareSampler(1000) // Sample for every 1000 milliseconds
log.SetSampler(tas) // Sample trace data
metric.SetSampler(tas) // Sample trace data
trace.SetSampler(tas) // Sample trace data
```
{% endcode %}

### Duration Aware Sampler

The Duration Aware Sampler enables you to sample data according to a given duration. For example, if the duration is set to 1,000, metric data will be sampled when Lambda duration is greater than 1,000 milliseconds. The configuration is shown below:

{% code title="Programmatic Configuration of DurationAwareSampler" %}
```go
import(
    "github.com/thundra-io/thundra-lambda-agent-go/samplers"
    "github.com/thundra-io/thundra-lambda-agent-go/metric"
    "github.com/thundra-io/thundra-lambda-agent-go/log"
	"github.com/thundra-io/thundra-lambda-agent-go/trace"   
)

// Sample if exc duration greather than 1000 milisecond
das := samplers.NewDurationAwareSampler(1000, true)
log.SetSampler(das)
metric.SetSampler(das)
trace.SetSampler(das)

```
{% endcode %}

### Error Aware Sampler

The Error Aware Sampler enables you to sample data according to erroneous invocations of a Lambda. For example, you should use this sampler if you want to send metric data only when a Lambda execution results with an error.

{% code title="Programmatic Configuration of ErrorAwareSampler" %}
```go
import(
    "github.com/thundra-io/thundra-lambda-agent-go/samplers"
    "github.com/thundra-io/thundra-lambda-agent-go/metric"
    "github.com/thundra-io/thundra-lambda-agent-go/log"
	"github.com/thundra-io/thundra-lambda-agent-go/trace"   
)

eas := NewErrorAwareSampler() // Sample data when Lambda fails
log.SetSampler(eas) // Sample log data
metric.SetSampler(eas) // Sample metric data
trace.SetSampler(eas) // Sample trace data
```
{% endcode %}

### Composite Sampler

You can combine one or more samplers with the help of the Composite Sampler, and you can also decide which logical operator (and, or) will be used when combining the samplers. Composite Sampler can be used in any plugin. Just give the Composite Sampler to the sampler configuration of one of Thundra’s agents’ plugin configurations. The example below shows how to configure a `CompositeSampler`, which in this instance combines `CountAwareSampler` and `ErrorAwareSampler` with an `and` operation.

{% code title="Programmatic Configuration of CompositeSampler" %}
```go
import(
    "github.com/thundra-io/thundra-lambda-agent-go/samplers"
    "github.com/thundra-io/thundra-lambda-agent-go/metric"
    "github.com/thundra-io/thundra-lambda-agent-go/log"
	"github.com/thundra-io/thundra-lambda-agent-go/trace"   
)

cas := samplers.NewCountAwareSampler(2) // Sample data every 5th invocation
eas := NewErrorAwareSampler() // Sample data when lamda fails

samplerArr := []samplers.Sampler{cas,eas}
cs := samplers.NewCompositeSampler(samplerArr, "and") // Sample data every 5th error

log.SetSampler(eas) // Sample log data
metric.SetSampler(eas) // Sample metric data
trace.SetSampler(eas) // sSample trace data
```
{% endcode %}
