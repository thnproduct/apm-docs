# AWS Lambda Debugging for Serverless Java

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

**Using the AWS Lambda Debugging with Java Lambda Functions**

1. Integrate Thundra’s Java agent with your Lambda function using **Lambda Layer** [integration options](integration-options/).
2. [Enable the AWS Lambda debugging](online-debugging.md#enabling-the-online-debugging) feature by adding the `THUNDRA_AGENT_LAMBDA_DEBUGGER_AUTH_TOKEN` environment variable.
3. [Specify a session name](../node.js/online-debugging.md#specifying-a-session-name) if needed (as long as you don't have multiple concurrent debugging sessions, you can use the default value and not specify a session name).
4. Configure runtime
   * **Java 8**
     * Change runtime to `Custom runtime`.
   * **Java 8 (Corretto)** and **Java 11 (Corretto)**
     * Set `AWS_LAMBDA_EXEC_WRAPPER` environment variable to `/opt/thundra_wrapper`
5. Configure Thundra Debugger in your IDE
   * See [IntelliJ](../ide-integrations/intellij-plugin.md) for more details. Lambda debugging for Java runtime is only available on IntelliJ for now.
6. Start the debug session in your IDE and invoke your function. Thundra Debugger will establish the connection between the IDE and your Lambda function using your authentication token and the session name.
7. You can [disable the AWS Lambda Debugging](online-debugging.md#disabling-the-online-debugging) once you complete your debugging session.

{% hint style="warning" %}
For longer debugging sessions, you will need to make sure to increase your function's timeout value when you enable online debugging.
{% endhint %}

{% hint style="info" %}
To use the online debugging feature, you need to use Thundra’s Java agent along with layer version `62` or higher.
{% endhint %}

### **Enabling the AWS Lambda Debugging**

The fastest way to enable the AWS Lambda Debugger for a Lambda function is to set the `THUNDRA_AGENT_LAMBDA_DEBUGGER_AUTH_TOKEN` environment variable to the authentication token that you receive from the Thundra console. By doing this, Thundra will start your function in debug mode and you will be able to connect your function from your IDE to start the debugging session.

### **Disabling the AWS Lambda Debugging**

The AWS Lambda Debugging feature is disabled by default. However, if you want to manually disable it, you can set the `THUNDRA_AGENT_LAMBDA_DEBUGGER_ENABLE` environment variable to `false`. This ensures that even if you have `THUNDRA_AGENT_LAMBDA_DEBUGGER_AUTH_TOKEN`, online debugging will be still disabled.

### **Specifying a session name**

In order to match the two ends of an online debugging session (your Lambda function invocation and your local IDE), Thundra uses session names. When you enable online debugging in your Lambda function, the session name is set to the predefined value “`default`.” If you want to use another session name, you can specify it using the `THUNDRA_AGENT_LAMBDA_DEBUGGER_SESSION_NAME` environment variable.

### Broker Host Selection

Thundra’s broker establishes communication between your Lambda function and IDE by receiving data from the Lambda function and then forwarding it to your IDE. To reduce overhead during this transmission process, you need to select a broker host that provides the shortest pathway between your Lambda and IDE. Similarly, the broker host needs to be close to your Lambda function or your IDE location.

Thundra provides a broker host in `Oregon` by default, but we also support different broker hosts (listed below). You can select a broker host and set it to the `THUNDRA_AGENT_LAMBDA_DEBUGGER_BROKER_HOST` environment variable to reduce latency during a debugging session.

* [debug.thundra.io](http://debug.thundra.io/) (us-west-2 - Oregon)
* [debug-us-east-1.thundra.io](http://debug-us-east-1.thundra.io/) (us-east-1 - N. Virginia)
* [debug-eu-west-2.thundra.io](http://debug-eu-west-2.thundra.io/) (eu-west-2 - London)
* [debug-ap-northeast-1.thundra.io](http://debug-ap-northeast-1.thundra.io/) (ap-northeast-1 - Tokyo)

### Concurrent Session Limit

The number of debugging sessions an account can perform is limited for now. Currently, you can start two concurrent debugging sessions at a time. You can contact us at [support@thundra.io](mailto:support@thundra.io) or via the intercom bubble on the right-hand bottom corner of the Thundra console if you want to increase your concurrent session limits.

### **Environment variables to configure AWS Lambda Debugging**

| **Environment Variable**                                     | **Description**                                                                                                                                                                                                                                |
| ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `THUNDRA_AGENT_LAMBDA_DEBUGGER_AUTH_TOKEN`                   | The authentication token you receive from the Thundra console. This field is required and there is no default value.                                                                                                                           |
| `THUNDRA_AGENT_LAMBDA_DEBUGGER_SESSION_NAME`                 | The unique session name that identifies a current debugging session from other concurrent debugging sessions. The default value is "`default`."                                                                                                |
| `THUNDRA_AGENT_LAMBDA_DEBUGGER_BROKER_HOST`                  | The broker host address to which Thundra connects to start a debugging session with your IDE. The default value is "`debug.thundra.io`."                                                                                                       |
| `THUNDRA_AGENT_LAMBDA_DEBUGGER_BROKER_PORT`                  | The broker port that Thundra uses to connect the broker. The default value is `444`.                                                                                                                                                           |
| `THUNDRA_AGENT_LAMBDA_DEBUGGER_WAIT_MAX`                     | The maximum amount of time in milliseconds that your function should wait until a debugging session request comes from the IDE. The default value is `60000`.                                                                                  |
| `THUNDRA_AGENT_LAMBDA_DEBUGGER_LOGS_ENABLE`                  | The boolean value that enables Thundra debugger logs. The default value is `false`.                                                                                                                                                            |
| <p><code>THUNDRA_AGENT_LAMBDA_DEBUGGER_ENABLE</code><br></p> | The boolean value that enables/disables the Thundra debugger. It can be set to `true` to enable the debugger (the authentication token environment variable should also be set), and it can be set to `false` to disable the Thundra debugger. |
