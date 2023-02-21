# Configuring Logs for Node.js

## Configuring Node.js Logging

### Enabling Log Plugin

By default, log monitoring is disabled, but it can be enabled by setting the relevant configuration parameters. These parameters include the `thundra_agent_lambda_log_disable` environment variable and the `disable_log` object initialization parameter.

#### Enabling log programmatically on agent initialization

{% code title="Programmatic Log Plugin Configuration" %}
```javascript
const thundra = require("@thundra/core")({ 
  logConfig: {
    enabled: false,
  },
});
```
{% endcode %}

#### Enabling log using environment variables

{% code title="Log Plugin Configuration via Environment Variables" %}
```yaml
thundra_apiKey: ${self:custom.thundraApiKey}
thundra_agent_lambda_log_disable: false
```
{% endcode %}

### Creating the Thundra Logger

In order to use Thundra's log support, you need to simply write “`require`” from the `@thundra/core` package, and use the `createLogger` method.

{% code title="Configure the Thundra Logger" %}
```javascript
const thundra = require('@thundra/core');
const logger = thundra.createLogger();
```
{% endcode %}

When configuring your loggers, you can also set tag names so you can easily identify any logs that your functions may generate. The default name when setting up your log is `default`, but this can be changed as shown below:

{% code title="Configure the Thundra Logger Name" %}
```javascript
const thundra = require('@thundra/core');
const logger = thundra.createLogger({
    loggerName: 'fooLogger'
});
```
{% endcode %}

### Using the Thundra Logger

Using Thundra's Node.js logger, you can easily log variables, exceptions, and warnings and access those logs in the Thundra Web Console rather than CloudWatch Logs.

You can log in two different ways:

Using the trace, debug, info, warn, error, and fatal methods. All these methods support the `printf`-like format, which is the same as Node.js's [util.format](https://nodejs.org/api/util.html#util\_util\_format\_format\_args).

```javascript
const thundra = require('@thundra/core');
const logger = thundra.createLogger();

exports.handler = thundra()((event, context, callback) => {
    logger.trace('Hello %s', 'Thundra');
    logger.debug('Hello %s', 'Thundra');
    logger.info('Hello %s', 'Thundra');
});
```

* Using the log method, as demonstrated in the code snippet shown below.

{% code title="Using the log method" %}
```javascript
const thundra = require("@thundra/core")
const logger = thundra.createLogger();

exports.handler = thundra()((event, context, callback) => {
    logger.log({
        level: "trace",
        message: "Hey, I am tracing",
    });
 
    //or directly use as
    logger.log("trace","Hey, I am %s","tracing");
});
```
{% endcode %}

###
