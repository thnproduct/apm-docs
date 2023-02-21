# Configuring Logs for Python SDK

## Configuring Python Log Support

In order to use Thundra's log support, you need to add `ThundraLogHandler` to your logger. You can add this handler by logging the configuration file or you can directly add this handler to your logger object.

### Configuring Log Support via Config File

This process involves adding logging environment variables to your config.ini file, as shown below:

{% code title="Configuring using log file" %}
```yaml
[loggers]
keys=root

[handlers]

keys=thundraHandler

[formatters]
keys=simpleFormatter

[logger_root]
level=NOTSET
handlers=thundraHandler

[handler_thundraHandler]
class=ThundraLogHandler
level=NOTSET
formatter=simpleFormatter
args=()

[formatter_simpleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
datefmt=
```
{% endcode %}

As can be seen from the configurations specified in the code snippet above, the Log Support parameters (such as level and handlers to monitor) are set. Moreover, you can also specify a format for how you would prefer to view your log message, specified as

`format=%(asctime)s - %(name)s - %(levelname)s - %(message)s.`

You can then import the config file by using the Python logging configurations, as shown in the code snippet below:

{% code title="Importing Config File" %}
```python
from thundra.thundra_agent import Thundra
thundra = Thundra()
import logging
import logging.config
from thundra.plugins.log.thundra_log_handler import ThundraLogHandler

logging.config.fileConfig('config.ini')

@thundra
def handler(event, context):
...
```
{% endcode %}

### Adding Logger Programmatically

To configure and add Log Support within your Python modules, you will first have to import the logging library and get a logging. You will then need to add the handler to the logging instance, as shown in the code snippet below:

{% code title="" %}
```python
from thundra.thundra_agent import Thundra
import logging
from thundra.plugins.log.thundra_log_handler import ThundraLogHandler

thundra = Thundra()

logger = logging.getLogger('handler')
logger.setLevel(logging.DEBUG)
handler = ThundraLogHandler()
logger.addHandler(handler)


@thundra
def handler(event, context):
    
    logger.error('This is an error log')
    response = {
        'message': 'Hello Thundra!'
    }
    return response
```
{% endcode %}

### Enabling Log Plugin

By default, log monitoring is disabled, but it can be enabled by setting the relevant configuration parameters. These parameters include the `thundra_agent_lambda_log_disable` environment variable and the `disable_log` object initialization parameter.

#### Enabling log programmatically on agent initialization

{% code title="Programmatic Log Plugin Configuration" %}
```python
thundra = Thundra(api_key=’my_api_key’, disable_log=False)
```
{% endcode %}

#### Enabling log using environment variables

{% code title="Log Plugin Configuration via Environment Variables" %}
```yaml
thundra_apiKey: ${self:custom.thundraApiKey}
thundra_agent_lambda_log_disable: false
```
{% endcode %}

