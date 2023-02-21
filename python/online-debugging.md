# AWS Lambda Debugging for Serverless Python

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

### Using **AWS Lambda Debugging** with Python Lambda Functions

1. Integrate Thundra’s Python agent with your Lambda function using one of the available [**integration options**](https://apm.docs.thundra.io/python/integration-options).
2. Add the `ptvsd` layer is shown below, changing the `${region}` parameter:`arn:aws:lambda:${region}:269863060030:layer:thundra-lambda-python-ptvsd-layer:1`
3. [Enable the AWS Lambda Debugging](online-debugging.md#enabling-the-online-debugging) by adding the `thundra_agent_lambda_debugger_auth_token` environment variable and setting `thundra_agent_lambda_debugger_enable` to `true`.
4. [Specify a session name](online-debugging.md#specifying-a-session-name) if needed (as long as you don't have multiple concurrent debugging sessions, you can use the default value and not specify a session name).
5. Configure Thundra Debugger in your IDE ([VSCode](../ide-integrations/vscode-plugin/) and others).
6. Start the debug session in your IDE and invoke your function. Thundra Debugger will establish the connection between the IDE and your Lambda function using your authentication token and the session name.
7. You can [disable the Online Debugging](online-debugging.md#disabling-the-online-debugging) once you complete your debugging session.

{% hint style="warning" %}
For longer debugging sessions, you will need to make sure to increase your function's timeout value when you enable online debugging.
{% endhint %}

{% hint style="info" %}
To use the online debugging feature, you need to use version 2.4.5 of Thundra’s Python agent along with layer version 20 or higher.
{% endhint %}

### Enabling the **AWS Lambda Debugging**

The fastest way to enable the AWS Lambda Debugging for a Lambda function is to set the `thundra_agent_lambda_debugger_auth_token` environment variable to the authentication token that you receive from the Thundra console, and then to set `thundra_agent_lambda_debugger_enable` to `true`. By doing this, Thundra will start your function in debug mode and you will be able to connect your function from your IDE to start the debugging session.

### Disabling the **AWS Lambda Debugging**

The AWS Lambda Debugging feature is disabled by default. However, if you want to manually disable it, you can set the `thundra_agent_lambda_debugger_enable` environment variable to `false`. This ensures that even if you have `thundra_agent_lambda_debugger_auth_token`, AWS Lambda Debugging will still be disabled.

### Specifying a session name

In order to match the two ends of an AWS Lambda Debugging session (your _Lambda function invocation_ and your _local IDE_), Thundra uses session names. When you enable AWS Lambda Debugging in your Lambda function, the session name is set to the predefined value "`default`." If you want to use another session name, you can specify it using the `thundra_agent_lambda_debugger_session_name` environment variable.

### Broker Host Selection

Thundra’s broker establishes communication between your Lambda function and your IDE debugger by receiving data from the Lambda function and then forwarding it to your IDE. To reduce overhead during this transmission process, you need to select a broker host that provides the shortest pathway between your Lambda and IDE. Similarly, the broker host needs to be close to your Lambda function or your IDE location.

Thundra provides a broker host in Oregon by default, but we also support different broker hosts (listed below). You can select a broker host and set it to the `thundra_agent_lambda_debugger_broker_host` environment variable to reduce latency during a debugging session.

* [debug.thundra.io](http://debug.thundra.io/) (us-west-2 - Oregon)
* [debug-us-east-1.thundra.io](http://debug-us-east-1.thundra.io/) (us-east-1 - N. Virginia)
* [debug-eu-west-2.thundra.io](http://debug-eu-west-2.thundra.io/) (eu-west-2 - London)
* [debug-ap-northeast-1.thundra.io](http://debug-ap-northeast-1.thundra.io/) (ap-northeast-1 - Tokyo)

### Concurrent Session Limit

The number of debugging sessions an account can perform is limited for now. Currently, you can start two concurrent debugging sessions at a time. You can contact us at [support@thundra.io](mailto:support@thundra.io) or via the intercom bubble on the right-hand bottom corner of the Thundra console if you want to increase your concurrent session limits.

#### Environment variables to configure AWS Lambda Debugging

| Environment Variable                         | Description                                                                                                                                                                                                                                        |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `thundra_agent_lambda_debugger_auth_token`   | The authentication token you receive from the Thundra console. This field is **required** and there is **no default value**.                                                                                                                       |
| `thundra_agent_lambda_debugger_session_name` | The unique session name that identifies a current debugging session from other concurrent debugging sessions. The default value is "**default**."                                                                                                  |
| `thundra_agent_lambda_debugger_broker_host`  | The broker host address to which Thundra connects to start a debugging session with your IDE. The default value is "**debug.thundra.io**."                                                                                                         |
| `thundra_agent_lambda_debugger_broker_port`  | The broker port that Thundra uses to connect the broker. The default value is **444.**                                                                                                                                                             |
| `thundra_agent_lambda_debugger_wait_max`     | The maximum amount of time in milliseconds that your function should wait until a debugging session request comes from the IDE. The default value is **60000.**                                                                                    |
| `thundra_agent_lambda_debugger_logs_enable`  | The Boolean value that enables Thundra debugger logs. The default value is  **false.**                                                                                                                                                             |
| `thundra_agent_lambda_debugger_enable`       | The Boolean value that enables/disables the Thundra debugger. It can be set to **true** to enable the debugger (the authentication token environment variable should also be set), and it can be set to **false** to disable the Thundra debugger. |
