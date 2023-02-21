# Sampling in .NET SDK

You can reduce the amount of data that the .NET agent sends to the Thundra web console by enabling sampling. This feature allows you to add built-in sampling rules to the metric, log, and trace plugins or provide a custom sampler and implement sampling logic yourself.

Check out this example project on [GitHub](https://github.com/thundra-io/thundra-examples-lambda-dotnet/tree/master/thundra-sampling-example/src/ThundraSamplingExample) showing how to use sampling.

{% tabs %}
{% tab title="Trace Sampling" %}
The trace plugin provides four built-in samplers, but you can also provide your own custom or composite sampler. Built-in samplers include:

* Count Aware Sampler
* Time Aware Sampler
* Error Aware Sampler
* Duration Aware Sampler
{% endtab %}

{% tab title="Log Sampling" %}
The log plugin provides three built-in samplers, but you can also provide your own custom or composite sampler. Built-in samplers include:

* Count Aware Sampler
* Time Aware Sampler
* Error Aware Sampler
{% endtab %}

{% tab title="Metric Sampling" %}
{% hint style="info" %}
Thundra enables default sampling for the metric plugin that takes into account the time and count. The default sampler for metrics works every 5 minutes or every 100 requests.
{% endhint %}

The metric plugin provides three built-in samplers, but you can also provide your own custom or composite sampler. Built-in samplers include:

* Count Aware Sampler
* Time Aware Sampler
* Error Aware Sampler
{% endtab %}
{% endtabs %}

### Count Aware Sampler

The Count Aware Sampler enables you to sample data with a count frequency. For example, if the count frequency is 500, data will be sampled with every 500 Lambda invocations.

```csharp
...
        public Function()
        {
            ThundraConfig config = ThundraConfigProvider.GetThundraConfig();
            config.LogConfig.Sampler = new CountAwareSampler(500);
            config.TraceConfig.Sampler = new CountAwareSampler(500);            
            config.MetricConfig.Sampler = new CountAwareSampler(500);         
        }
...
```

### Time Aware Sampler

The Time Aware Sampler enables you to sample data with a time frequency. For example, if the time frequency is 300,000, data will be sampled every 300 seconds within consecutive Lambda invocations.

```csharp
...
        public Function()
        {
            ThundraConfig config = ThundraConfigProvider.GetThundraConfig();
            config.LogConfig.Sampler = new TimeAwareSampler(300000);
            config.TraceConfig.Sampler = new TimeAwareSampler(300000);            
            config.MetricConfig.Sampler = new TimeAwareSampler(300000);         
        }
...
```

### Error Aware Sampler

The Error Aware Sampler enables you to sample data according to erroneous invocations of a Lambda. For example, you would use this sampler if you want to send trace data only if a Lambda execution results with an error.

```csharp
...
        public Function()
        {
            ThundraConfig config = ThundraConfigProvider.GetThundraConfig();
            config.LogConfig.Sampler = new ErrorAwareSampler();
            config.TraceConfig.Sampler = new ErrorAwareSampler();            
            config.MetricConfig.Sampler = new ErrorAwareSampler();         
        }
...
```

### Duration Aware Sampler

The Duration Aware Sampler enables you to sample data according to the duration of a Lambda. For example, you would use this sampler if you want to send trace data only if the duration of a Lambda is longer than 500 milliseconds.

```csharp
...
        public Function()
        {
            ThundraConfig config = ThundraConfigProvider.GetThundraConfig();
            config.LogConfig.Sampler = new DurationAwareSampler(5000, true);
            config.TraceConfig.Sampler = new DurationAwareSampler(5000, true);            
            config.MetricConfig.Sampler = new DurationAwareSampler(5000, true);
        }
...
```

### Composite Sampler

The Composite Sampler enables you to compose multiple samplers using a specified composition operation (`and` or `or`).

An example using `TimeAwareSampler` and `CountAwareSampler` with or can be seen below:

```csharp
...
        public Function()
        {
            ThundraConfig config = ThundraConfigProvider.GetThundraConfig();
            config.LogConfig.Sampler = new CompositeSampler(CompositeSampler.SamplerCompositionOperator.OR,
                            new List<Sampler> {
                                new TimeAwareSampler(300000),
                                new CountAwareSampler(100)
                                }
                            );        
            config.TraceConfig.Sampler = new CompositeSampler(CompositeSampler.SamplerCompositionOperator.OR,
                            new List<Sampler> {
                                new TimeAwareSampler(300000),
                                new CountAwareSampler(100)
                                }
                            );            
            config.MetricConfig.Sampler = new CompositeSampler(CompositeSampler.SamplerCompositionOperator.OR,
                            new List<Sampler> {
                                new TimeAwareSampler(300000),
                                new CountAwareSampler(100)
                                }
                            );
        }
...
```

###

