# Reducing Data Size with Sampling

To have a reliable serverless system, being aware of issues and being able to fix them quickly is crucial. Observability is key to providing useful contextual information about how your system is working, detecting problems, and taking action quickly. However, collecting logs and data to observe your serverless system can cause cost issues – but this isn’t a problem with Thundra’s sampling feature.&#x20;

Data in your system is collected asynchronously or synchronously, which means an immense amount of data is sent to CloudWatch logs and results in a higher cost. Sampling allows you to easily reduce your cost and continue to observe your serverless system while catching any issues that occur. You can sample metrics, traces, and logs, but taking a percentage of your invocation can be enough to observe metrics and anomalies and debug your system.&#x20;

### Sampling Metrics

When sampling your metric data, you have five different options:

* Count Aware Sampler - samples metrics for a configured percentage of all invocations.
* Time Aware Sampler - samples metrics with every configured time within consecutive Lambda invocations.
* Error Aware Sampler - samples metrics for erroneous invocations.
* Custom Sampler - samples metrics with customized functions.
* Composite Sampler - samples metrics with more than one sampler.

### Sampling Traces

In Thundra, you can also sample your trace data with the following options:

* Duration Aware Sampler - samples trace data only if the duration of an invocation is more than the configured time.
* Error Aware Sampler - samples traces for erroneous invocations.
* Count Aware Sampler - samples traces for a configured percentage of all invocations.
* Time Aware Sampler - samples trace data with every configured time within consecutive Lambda invocations.
* Custom Sampler - samples traces with a customized function.

### Sampling Logs

To sample your logs, you have the following options:

* Count Aware Sampler - samples logs for a configured percentage of all invocations.
* Time Aware Sampler - samples logs with every configured time within consecutive Lambda invocations.
* Error Aware Sampler - samples logs for erroneous invocations.
* Custom Sampler - samples logs with customized functions.
* Log Level Filtering - sets the level of logs that will be reported.

### How to configure your serverless system for sampling?

For more information about using sampling in your serverless system, visit the pages listed below:

* [Node.js](../node.js/nodejs-configuration-options/sampling-support.md)
* [Python](../python/configuration-options/sampling-support.md)
* [Java](../java/configuration-options/sampling.md)
* [Go](../go/go-configuration-options/sampling-support.md)
* [.NET](../.net/configuration-options/sampling.md)

####
