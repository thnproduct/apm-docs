# Configuring Logs for Java SDK

If you use Thundra’s Java layer or a custom runtime, the log plugin is disabled by default, but you can enable it by setting the `thundra_agent_lambda_log_disable` environment variable to `false`.

#### Capturing Standard Output/Error logs

By default, all of the standard output/error logs are captured and printed during the invocation. To disable this behavior, you can set the `thundra_agent_lambda_log_console_disable` environment variable to `true`.

#### Capturing Lambda Context logs

By default, all of the logs over `context.getLogger()` are captured and printed during the invocation. To disable this behavior, you can set the `thundra_agent_lambda_log_context_disable` environment variable to `true`.

Thundra also provides log monitoring over `log4j1` and `log4j2` logging frameworks.

## Log4j1 Support

Thundra provides `io.thundra.agent.log.integrations.log4j1.ThundraAppender` (an `org.apache.log4j.Appender` implementation), which reports logs to our system and associates them with the active span and trace if available (tracing is enabled).

To monitor your logs through `org.apache.log4j.Logger,` you need to define and configure Thundra’s `io.thundra.agent.log.integrations.log4j1.ThundraAppender` as any regular appender implementation in the log4j configuration.

The following example declarative configurations show how to configure `log4j` for monitoring logs.

{% code title="log4j.properties" %}
```typescript
log4j.rootLogger=INFO, thundra
log4j.appender.thundra=io.thundra.agent.log.integrations.log4j1.ThundraAppender
log4j.appender.thundra.layout=org.apache.log4j.PatternLayout
log4j.appender.thundra.layout.ConversionPattern=%d %t %p [%c{4}] %m%n
log4j.logger.io.thundra.lambda.demo=TRACE
```
{% endcode %}

{% code title="log4j.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd"> 
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'> 
    <root> 
        <level value="TRACE"/> 
        <appender-ref ref="thundra"/> 
    </root>
    <appender name="thundra" 
              class="io.thundra.agent.log.integrations.log4j1.ThundraAppender"> 
        <layout class="org.apache.log4j.PatternLayout"> 
            <param name="ConversionPattern" value="%d %t %p [%c{4}] %m%n"/>
        </layout> 
    </appender> 
    <logger name="io.thundra.lambda.demo"> 
        <level value="TRACE"/> 
    </logger>
</log4j:configuration>
```
{% endcode %}

Then, `io.thundra.agent.log.integrations.log4j1.ThundraAppender` reports log events from appended logger to Thundra.

{% code title="UserService.java" %}
```java
...

import org.apache.log4j.Logger;

...


public class 
 {

    private final Logger logger = Logger.getLogger(getClass());

    ...

    public User get(String id) {
        logger.info(String.format("User with id %s has been requested", id));

        ...

        if (user == null) { 
            logger.warn(String.format(
                "User with id %s could not be found for retrieving", id)); 
        } else { 
            logger.info(String.format(
                "User with id %s has been retrieved: %s", id, user)); 
        } 
        return user;
    }

    ...
}
```
{% endcode %}

After we logged through with Thundra’s appender, we can see the logs in the span of owner method where the logs are printed.

## Log4j2 Support

Thundra provides `io.thundra.agent.log.integrations.log4j2.ThundraAppender` (an `org.apache.logging.log4j.core.Appender` implementation), which reports logs to our system and associates them with the active span and trace if available (tracing is enabled).

To monitor your logs through `org.apache.logging.log4j.Logger`, you need to define and configure Thundra’s `io.thundra.agent.log.integrations.log4j2.ThundraAppender` as any regular appender implementation in the log4j configuration.

The following example declarative configurations show how to configure `log4j` for monitoring logs.

{% code title="log4j2.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<Configuration packages="io.thundra.agent.log.integrations.log4j2">
    <Appenders>
        <Thundra>
            <PatternLayout pattern="%msg"/>
        </Thundra>
    </Appenders>
    <Loggers>
        <Root level="info">
            <AppenderRef ref="Thundra"/>
        </Root>
    </Loggers>
</Configuration>
```
{% endcode %}

Then, `io.thundra.agent.log.integrations.log4j2.ThundraAppender` reports log events from appended logger to Thundra.

{% code title="UserService.java" %}
```java
...


import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

...

public class UserService {

    private final Logger logger = LogManager.getLogger(getClass());

    ...

    public User get(String id) {
        logger.info(String.format("User with id %s has been requested", id));

        ...

        if (user == null) { 
            logger.warn(String.format(
                "User with id %s could not be found for retrieving", id)); 
        } else { 
            logger.info(String.format(
                "User with id %s has been retrieved: %s", id, user)); 
        } 
        return user;
    }

    ...
}
```
{% endcode %}

