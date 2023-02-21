# Sampling in Node.js SDK

You can reduce the amount of data that the Node.js agent sends to the Thundra Web Console by enabling sampling. You can either add built-in sampling rules to the Metric and Trace plugins, or you can provide a custom sampler and implement the sampling logic by yourself. Do note, however, that you can only sample timed-out invocations. To see an example, take a look at the project on [GitHub](https://github.com/thundra-io/thundra-examples-lambda-nodejs/tree/master/sampling-demo) for more examples.

## Timed-Out Invocation Sampling

You can only send data to Thundra when a Lambda time-out happens. With timed-out sampling, you can focus on your timed-out invocations. The code below shows how to enable timed-out invocations programmatically:

{% code title="Sampling timed-out invocations" %}
```javascript
const config = {
	 sampleTimedOutInvocations: true
};

const thundra = require("@thundra/core");

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can also enable timed-out sampling by setting `thundra_agent_lambda_sample_timed_out_invocations` to `true` without any code change.

## Masking Stack Traces

To reduce your data usage, you can mask the stack traces of errors that the Thundra Node.js agent collects. You can enable masking of stack traces by setting the environment variable `thundra_agent_lambda_error_stacktrace_mask` to “`true`” without any code change.

## Composite Sampler

You can combine one or more samplers with the help of Composite Sampler, which can be used in any plugin. You are also to decide which logical operator (AND, OR) will be used when combining samplers. All you need to do is give the Composite Sampler to the sampler configuration of one of the Thundra agents’ plugin configurations. The example below shows how to configure a `CompositeSampler`, which combines the `CountAwareSampler` and `TimeAwareSampler` with an `AND` operation. The `CompositeSampler` is given to the trace plugin.

{% code title="Composite sampler example" %}
```javascript
const thundra = require("@thundra/core");

// Imports
const CompositeSampler = thundra.samplers.CompositeSampler;
const CountAwareSampler = thundra.samplers.CountAwareSampler;
const TimeAwareSampler = thundra.samplers.TimeAwareSampler;
const SamplerCompositionOperator = thundra.samplers.SamplerCompositionOperator;

// Init two separate samplers
const sampler1 = new CountAwareSampler(2); // Samples every 2th.
const sampler2 = new TimeAwareSampler(3000); // Samples every 3 seconds.

// Create a composite sampler with count and time aware and combine them with AND // operator, default is OR

const sampler = new CompositeSampler([sampler1, sampler2], SamplerCompositionOperator.AND)

const compositeConfig = {
    traceConfig: {
        sampler
    },
};

exports.handler = thundra(compositeConfig)((event, context, callback) => {
    callback(null, {msg: event.msg});
});
```
{% endcode %}

## Metric Sampling

The Metric plugin provides three built-in samplers, and you can also use a custom or composite sampler. These samplers include:

* Count Aware Sampler
* Time Aware Sampler
* Error Aware Sampler
* Custom Sampler

### Count Aware Sampler

The Count Aware Sampler enables you to sample metric data with a count frequency. For example, if the count frequency is 50, metric data will be sampled with every 50 Lambda invocations. Here is the configuration:

{% code title="Count Aware Metric Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const CountAwareSampler = thundra.samplers.CountAwareSampler;

const config = {
   metricConfig: {
      sampler: new CountAwareSampler(50) // Sample metrics every 50th invocation
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can update count frequency without re-deploying Lambda functions. After enabling Count Aware Sampler with a programmatic configuration, change the count frequency by setting the `thundra_agent_lambda_sampler_countAware_countFreq` environment variable in AWS Lambda.

### Time Aware Sampler

The Time Aware Sampler enables you to sample metric data with a time frequency. For example, if the time frequency is 300,000, metric data will be sampled every 300 seconds within consecutive Lambda invocations. Here is the configuration:

{% code title="Time Aware Metric Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const TimeAwareSampler = thundra.samplers.TimeAwareSampler;

const config = {
   metricConfig: {
      sampler: new TimeAwareSampler(300000) // Sample metrics every 300s
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can update time frequency without re-deploying Lambda functions. After enabling Time Aware Sampler with a programmatic configuration, change the time frequency by setting the `thundra_agent_lambda_sampler_timeAware_timeFreq` environment variable in AWS Lambda.

### Error Aware Sampler

The Error Aware Sampler enables you to sample metric data according to erroneous invocations of a Lambda function. For example, you can send metric data from only the Lambda execution that results in an error.

{% code title="Error Aware Metric Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const ErrorAwareSampler = thundra.samplers.ErrorAwareSampler;

const config = {
   metricConfig: {
      sampler: new ErrorAwareSampler() // Sample metrics when Lambda fails
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

### Custom Sampler

You also have the ability to implement a custom sampler. A custom sampler is an Object with the `isSampled` function. The `isSampled` function should return “`true`” if you want to send metric data to Thundra, and should return “`false`” if you do not want to send the data.

{% code title="Custom metric sampler example" %}
```javascript
const customSamplerConfig = {
   metricConfig: {
        sampler: {
            isSampled: () => {
                // Decide what to do yourself here. return true/false.
                return true;
            }
        }
    }, 
};

const thundra = require("@thundra/core");

exports.handler = thundra(customSamplerConfig)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

## Trace Sampling

The Trace plugin provides four built-in samplers, and you can also use a custom or composite sampler. These samplers include:

* Duration Aware Sampler
* Error Aware Sampler
* Count Aware Sampler
* Time Aware Sampler
* Custom Sampler

### Duration Aware Sampler

The Duration Aware Sampler enables you to sample trace data according to the duration of a Lambda function. For example, you can send trace data only if the duration of the Lambda function is longer than 500 milliseconds. Here is the configuration:

{% code title="Duration Aware Trace Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const DurationAwareSampler = thundra.samplers.DurationAwareSampler;

const config = {
   traceConfig: {
      sampler: new DurationAwareSampler() // Sample traces when Lambda fails
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

### Error Aware Sampler

The Error Aware Sampler enables you to sample trace data according to erroneous invocations of the Lambda function. For example, you can send trace data from only the Lambda execution that results in an error.

{% code title="Error Aware Trace Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const ErrorAwareSampler = thundra.samplers.ErrorAwareSampler;

const config = {
   traceConfig: {
      sampler: new ErrorAwareSampler() // Sample traces when Lambda fails
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

### Count Aware Sampler

The Count Aware Sampler enables you to sample trace data with a count frequency. For example, if the count frequency is 50, trace data will be sampled with every 50 Lambda invocations. Here is the configuration:

{% code title="Count Aware Trace Sampler" %}
```javascript
const thundra = require("@thundra/core");

const CountAwareSampler = thundra.samplers.CountAwareSampler;

const config = {
   traceConfig: {
      sampler: new CountAwareSampler(50) // Sample traces every 50th invocation
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can update count frequency without re-deploying Lambda functions. After enabling Count Aware Sampler with a programmatic configuration, change the count frequency by setting the `thundra_agent_lambda_sampler_countAware_countFreq` environment variable in AWS Lambda.

### Time Aware Sampler

The Time Aware Sampler enables you to sample trace data with a time frequency. For example, if the time frequency is 300,000, trace data will be sampled every 300 seconds within consecutive Lambda invocations. Here is the configuration:

{% code title="Time Aware Trace Sampler" %}
```javascript
const thundra = require("@thundra/core");

const TimeAwareSampler = thundra.samplers.TimeAwareSampler;

const config = {
    traceConfig: {
      sampler: new TimeAwareSampler(300000) // Sample traces every 300s
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can update time frequency without re-deploying Lambda functions. After enabling Time Aware Sampler with a programmatic configuration, change the time frequency by setting the `thundra_agent_lambda_sampler_timeAware_timeFreq`' environment variable in AWS Lambda.

### Root Aware Sampler

The Root Aware Sampler enables you to sample trace data with a type of span. In this case, every span will be sampled except root span. Here is the configuration:

{% code title="Root Aware Sampler" %}
```javascript
const thundra = require("@thundra/core");

const RootAwareSampler = thundra.samplers.RootAwareSampler;

const config = {
    traceConfig: {
      sampler: new RootAwareSampler() // Sample spans except root span
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

### Custom sampler

You also have the ability to implement a custom sampler. A custom sampler is an Object with the `isSampled` function. The `isSampled` function should return “`true`” if you want to send trace data to Thundra, and should return “`false`” if you do not want to send the data.

{% code title="Custom trace sampler example" %}
```javascript
const customSamplerConfig = {
   traceConfig: {
        sampler: {
            isSampled: (span) => {
                // Decide what to do yourself here. return true/false.
                return true;
            }
        }
    }, 
};

const thundra = require("@thundra/core");

exports.handler = thundra(customSamplerConfig)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

## Log Sampling

The Log plugin provides three built-in samplers, and you can also use a custom or composite sampler. These samplers include:

* Error Aware Sampler
* Count Aware Sampler
* Time Aware Sampler
* Custom Sampler
* Log Level Filtering

### Error Aware Sampler

The Error Aware Sampler enables you to sample log data according to erroneous invocations of the Lambda function. For example, you can send log data from only the Lambda execution that results in an error.

{% code title="Error Aware Log Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const ErrorAwareSampler = thundra.samplers.ErrorAwareSampler;

const config = {
   logConfig: {
      sampler: new ErrorAwareSampler() // Sample logs when Lambda fails
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

### Count Aware Sampler

The Count Aware Sampler enables you to sample log data with a count frequency. For example, if the count frequency is 50, log data will be sampled with every 50 Lambda invocations. Here is the configuration:

{% code title="Count Aware Log Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const CountAwareSampler = thundra.samplers.CountAwareSampler;

const config = {
   logConfig: {
      sampler: new CountAwareSampler(50) // Sample logs every 50th invocation
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can update count frequency without re-deploying Lambda functions. After enabling Count Aware Sampler with a programmatic configuration, you can change the count frequency by setting the `thundra_agent_lambda_sampler_countAware_countFreq` environment variable in AWS Lambda.

### Time Aware Sampler

The Time Aware Sampler enables you to sample log data with a time frequency. For example, if the time frequency is 300,000, log data will be sampled every 300 seconds within consecutive Lambda invocations. Here is the configuration:

{% code title="Time Aware Log Sampler example" %}
```javascript
const thundra = require("@thundra/core");

const TimeAwareSampler = thundra.samplers.TimeAwareSampler;

const config = {
    logConfig: {
      sampler: new TimeAwareSampler(300000) // Sample logs every 300s
    }
};

exports.handler = thundra(config)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

You can update time frequency without re-deploying Lambda functions. After enabling Time Aware Sampler with a programmatic configuration, change the time frequency by setting the `thundra_agent_lambda_sampler_timeAware_timeFreq`' environment variable in AWS Lambda.

### Custom sampler

You also have the ability to implement a custom sampler. A custom sampler is an Object with the `isSampled` function. The `isSampled` function should return “`true`” if you want to send trace data to Thundra, and should return “`false`” if you do not want to send the data.

{% code title="Custom Log sampler example" %}
```javascript
const customSamplerConfig = {
   logConfig: {
        sampler: {
            isSampled: () => {
                // Decide what to do yourself here. return true/false.
                return true;
            }
        }
    }, 
};

const thundra = require("@thundra/core");

exports.handler = thundra(customSamplerConfig)((event, context, callback) => {
    callback(null, {msg : 'hello world'});    
});
```
{% endcode %}

### Log Level Filtering

The following are Thundra Log Levels in increasing precedence: `Trace`, `debug`, `info`, `warn`, `error`, and `fatal`. You can set the log level by setting the environment variable `thundra_agent_lambda_log_loglevel` to one of the following:

* Trace
* debug
* info
* warn
* error
* fatal
* none

For instance, if `thundra_agent_lambda_log_loglevel` is debug, only debug and higher precedence logs will be reported. None of the logs lower in precedence will be reported, such as Trace.

