# AWS Lambda Debugging for Serverless Node.js

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

### **Using the** AWS Lambda Debugging **with Node.js Lambda Functions**

1. Integrate Thundra’s Node.js agent with your Lambda function using one of the available [integration options](nodejs-integration-options.md).
2. [Enable the AWS Lamda debugging](online-debugging.md#enabling-the-online-debugging) by adding the `thundra_agent_lambda_debugger_auth_token` environment variable.
3.  [Specify a session name](online-debugging.md#specifying-a-session-name) if needed (as long as you don't have multiple concurrent debugging sessions, you can use the default value and not specify a session name).

    Configure Thundra Debugger in your IDE ([VSCode](../ide-integrations/vscode-plugin/), [IntelliJ](../ide-integrations/intellij-plugin.md) and Other)
4. Start the debug session in your IDE and invoke your function. Thundra Debugger will establish the connection between the IDE and your Lambda function using your authentication token and the session name.
5. You can [disable the AWS Lambda Debugging](online-debugging.md#disabling-the-online-debugging) once you complete your debugging session.

{% hint style="warning" %}
For longer debugging sessions, you will need to make sure to increase your function's timeout value when you enable AWS Lambda Debugging.
{% endhint %}

{% hint style="info" %}
To use the AWS Lambda Debugging debugging feature, you need to use version 2.9.1 of Thundra’s Node.js agent along with layer version 42 or higher.
{% endhint %}

### Using the AWS Lambda Debugging Debugging with TypeScript

A small configuration is needed in order to work with TypeScript files.&#x20;

Before starting a session with our [VSCode](../ide-integrations/vscode-plugin/) plugin, select `Thundra: Change local root` command in the menu and point to your build folder where you keep your `.js` and `.js.map` files.&#x20;

{% hint style="info" %}
See more details about changing the local root [here](../ide-integrations/vscode-plugin/changing-local-root.md).
{% endhint %}

Another thing to make sure of is that to match your local build folder structure to the remote. Unfortunately, it's hard to make assumptions about the user's folder structure. So, it's best to make this manually at the moment. Putting `"rootDir": "."` in your `.tsconfig` file should compile your files to match the folder structure.&#x20;

For example, if your code is in `src/api/hello.ts` file, AWS Lambda will put the compiled code under `src/api/hello.js` **** and specifying the root directory in your `.tsconfig` will compile your code under, let's say `build/src/api/hello.js` .&#x20;

By changing the local root of our plugin to `./build`, you are ready to start debugging your function.

{% hint style="warning" %}
If you use any module bundler solutions like **Webpack**, AWS Lambda Debugging debugging features for both JavaScript and TypeScript will not work properly with bundled files. \
\
You need to set`keepOutputDirectory: true` to upload your source files to the remote and match your local setup with it to start debugging.
{% endhint %}

#### **Enabling the** AWS Lambda Debugging **Debugging**

The fastest way to enable the Thundra Debugger for a Lambda function is to set the `thundra_agent_lambda_debugger_auth_token` environment variable to the authentication token that you receive from the Thundra console. By doing this, Thundra will start your function in debug mode and you will be able to connect your function from your IDE to start the debugging session.

#### **Disabling the** AWS Lambda Debugging **Debugging**

The AWS Lambda Debugging feature is disabled by default. However, if you want to manually disable it, you can set the `thundra_agent_lambda_debugger_enable` environment variable to false. This ensures that even if you have `thundra_agent_lambda_debugger_auth_token`, AWS Lambda Debugging will still be disabled.

#### **Specifying a session name**

In order to match the two ends of an AWS Lambda Debugging session (your Lambda function invocation and your local IDE), Thundra uses session names. When you enable online debugging in your Lambda function, the session name is set to the predefined value`default`. If you want to use another session name, you can specify it using the `thundra_agent_lambda_debugger_session_name` environment variable.

#### Broker Host Selection

Thundra’s broker establishes communication between your Lambda function and your IDE debugger by receiving data from the Lambda function and then forwarding it to your IDE. To reduce overhead during this transmission process, you need to select a debugger that provides the shortest pathway between your Lambda and IDE. Similarly, the broker host needs to be close to your Lambda function or your IDE location.

Thundra provides a broker host in Oregon by default, but we also support different broker hosts (listed below). You can select a broker host and set it to the `thundra_agent_lambda_debugger_broker_host` environment variable to reduce latency during a debugging session.

* [debug.thundra.io](http://debug.thundra.io/) (us-west-2 - Oregon)
* [debug-us-east-1.thundra.io](http://debug-us-east-1.thundra.io/) (us-east-1 - N. Virginia)
* [debug-eu-west-2.thundra.io](http://debug-eu-west-2.thundra.io/) (eu-west-2 - London)
* [debug-ap-northeast-1.thundra.io](http://debug-ap-northeast-1.thundra.io/) (ap-northeast-1 - Tokyo)

#### Concurrent Session Limit

The number of debugging sessions an account can perform is limited for now. Currently, you can start two concurrent debugging sessions at a time. Contact us at [support@thundra.io](mailto:support@thundra.io) if you want to increase your concurrent session limits.

#### **Environment variables to configure** AWS Lambda Debugging

| **Environment Variable**                                     | **Description**                                                                                                                                                                                                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `thundra_agent_lambda_debugger_auth_token`                   | <p></p><p>The authentication token you receive from the Thundra console. This field is required and there is no default value.</p>                                                                                                          |
| `thundra_agent_lambda_debugger_session_name`                 | The unique session name that identifies a current debugging session from other concurrent debugging sessions. The default value is "default."                                                                                               |
| `thundra_agent_lambda_debugger_broker_host`                  | The broker host address to which Thundra connects to start a debugging session with your IDE. The default value is `debug.thundra.io`.                                                                                                      |
| `thundra_agent_lambda_debugger_broker_host`                  | The broker port that Thundra uses to connect the broker. The default value is 444.                                                                                                                                                          |
| `thundra_agent_lambda_debugger_wait_max`                     | The maximum amount of time in milliseconds that your function should wait until a debugging session request comes from the IDE. The default value is 60000.                                                                                 |
| `thundra_agent_lambda_debugger_logs_enable`                  | The Boolean value that enables Thundra debugger logs. The default value is false.                                                                                                                                                           |
| <p><code>thundra_agent_lambda_debugger_enable</code><br></p> | The Boolean value that enables/disables  the Thundra debugger. It can be set to true to enable the debugger (the authentication token environment variable should also be set), and it can be set to false to disable the Thundra debugger. |
