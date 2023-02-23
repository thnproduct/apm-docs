# Configuring Logs for .NET SDK

{% hint style="danger" %}
The Thundra .NET agent's log plugin is disabled on version 1.5.0. The Thundra console now receives log data through CloudWatch integration. Older agent versions can continue to send log data, but it is recommended to use the latest version and add the integration. Read [here](../../thundra-web-console/profile/settings-page/aws-tab/) to learn how to set up CloudWatch integration.
{% endhint %}

Thundra's .NET agent provides two modes of logging. The simple way to log is via the console using the `Console` method of C#. Therefore, `Console.WriteLine("Hello")` would log the message `“hello”` to the Thundra console.

The second way to log messages to the console involves using the **Microsoft Logging extension**, where you use `LoggerFactory` and add the Thundra provider as described in the following sections.

{% hint style="info" %}
Microsoft.Extensions.Logging version 2.2.0+ are supported.
{% endhint %}

## Initializing Thundra Logger

Thundra Logger is built on top of [Microsoft's logging extension](https://www.nuget.org/packages/Microsoft.Extensions.Logging/) with ASP.NET. Therefore, when you create a new instance of `LoggerFactory()`, you must add the Thundra logger as well: `LoggerFactory().AddThundraProvider()`. After that, you may create a logger by using the `CreateLogger<FunctionName>()` method of the Logger Factory and then start to log with the levels present.

According to the [LoggerExtension](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.loggerextensions?view=aspnetcore-3.1) the following levels are present:

* LogCritical
* LogDebug
* LogError
* LogInformation
* LogTrace
* LogWarning

The code snippet below is a basic example illustrating how you can set up Thundra's logger.

```csharp
var loggerFactory = new LoggerFactory().AddThundraProvider();
var logger = loggerFactory.CreateLogger<Function>();
```

## Configuring Logger

There are three configurations that you can set in different ways. The configurations are:

* Setting the minimum log level
* Disabling console logging
* Disabling log data completely

Configuration of .NET logs can be done in three different ways. These involve programmatic configurations, configuration via environment variables, and user configuration files. All log data is enabled by default, and the minimum log level is `Trace` with priority level 0.

```csharp
ThundraConfig config = ThundraConfigProvider.GetThundraConfig();
config.DisableLog = "false";
config.LogLevel = "trace"
config.DisableConsoleLog = "true"
```

### Changing Log Level

You can change log levels using the environment variable `thundra_agent_lambda_log_disable` or by programmatically changing ThundraConfig's `LogLevel` parameter.

### Disabling Console Logs

You can disable console logs using the environment variable `thundra_agent_lambda_log_console_disable` or by programmatically changing ThundraConfig's `DisableLog` parameter.

### Enabling Log Plugin

Log monitoring is disabled by default, but can be enabled by setting the relevant configuration parameters. These parameters include the `thundra_agent_lambda_log_disable` environment variable and the `disable_log` object initialization parameter.
