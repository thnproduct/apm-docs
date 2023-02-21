# Others

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

### Thundra Debug Client

Thundra provides plugins for [VSCode(Nodejs, Python)](https://docs.thundra.io/ide-integrations/vscode-plugin) and [ Intellij Idea (Java).](https://docs.thundra.io/ide-integrations/intellij-plugin) The Thundra Debug Client can be used for other development environments by supporting remote debugging and acting as a proxy between the local environment and the Thundra Debug Broker.

In the information below, you can learn how to use the Thundra client to debug your Lambda functions on other IDEs. Following the Installation section, see the examples shown for [Eclipse](others.md#eclipse) and [WebStorm](others.md#webstorm) to understand how to use Thundra Debugger Client.

#### Installation of Thundra Debug Client

To install globally run:

```bash
$ sudo pip install thundra-debug-client
```

or to install for current user:

```bash
$ pip install thundra-debug-client --user 
```

#### Synopsis

```bash
thundradebug [--help] [-h HOST] [-p PORT] [-a AUTH] [-f FILE]
                    [-r PROFILE] [-v] [-sp KEY=VALUE [KEY=VALUE ...]]
```

#### Description

| Options                     | Description                                                                                                                                                                                                                                  |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`-h`**, **`--host`**      | Debug broker host                                                                                                                                                                                                                            |
| **`-p`**, **`--port`**      | Debug broker port                                                                                                                                                                                                                            |
| **`-a`**, **`--auth`**      | Authentication token                                                                                                                                                                                                                         |
| **`-f`**, **`--file`**      | Configuration file path                                                                                                                                                                                                                      |
| _**`-r`**, **`--profile`**_ | Configuration profile name                                                                                                                                                                                                                   |
| **`-sp`**                   | <p>KEY=VALUE [KEY=VALUE ...] <br>Session port mappings in <code>-sp &#x3C;port>=&#x3C;session-name></code> format</p><p>example: <code>-sp 12358=default</code></p><p>For each session name, a different port number should be provided.</p> |

{% hint style="info" %}
In order to match the two ends of an online debugging session (your Lambda function invocation and your local IDE), Thundra uses session names. When you enable online debugging in your Lambda function, the session name is set to the predefined value:  "**default**". If you want to use another session name, you can specify it using the `thundra_agent_lambda_debugger_session_name` environment variable.&#x20;
{% endhint %}

#### Configuration File Format

Configuration parameters to thundra debug cli can be provided by either configuration files or cli arguments. Default configuration is stored under `.thundra/debug-client.json` in the user home directory. Another configuration file path can be given to cli by `-f` or `--file` arguments. The configuration file format is as follows:

```bash
{
    "profiles": {
        "default": {
            "debugger": {
                "authToken": <set-your-thundra-auth-token>,
                "sessionName": "default",
                "brokerHost": "debug.thundra.io",
                "brokerPort": 443,
                "sessions" : {
                  "default" : 12358
                }
            }
        }
    }
}
```

### Eclipse

* Install [Thundra Debug Client](https://docs.thundra.io/ide-integrations/others#installation-of-thundra-debug-client).
* Open the Eclipse IDE and click on **`Debug Configurations`**.

![](<../.gitbook/assets/Screen Shot 2020-03-02 at 01.02.04.png>)

* Select **`Remote Java Application`** and add a new configuration.

![](<../.gitbook/assets/Screen Shot 2020-03-02 at 00.55.51.png>)

* Select “Project” and enter a port number to which the Thundra Debug Client will listen. Then click **`Apply`**.

![](<../.gitbook/assets/Screen Shot 2020-03-02 at 01.00.06.png>)

{% hint style="info" %}
When starting the Thundra Debug Client, the port number configured here should also be given as either a command line argument or in the configuration file. For example, if you want to map the session name lab to the port 1111, the client should be started as:

`thundradebug -sp 1111=lab`
{% endhint %}

* Start the Thundra Debug Client provided with the required parameters (authorization token and port-to-session name mapping if you are not using default mapping `12358=default`).

&#x20;    Example:

```bash
$ thundradebug --auth <your-auth-token> -sp 1111=lab 12358=default
```

* Click **`Debug`** on the configuration tab.

![](<../.gitbook/assets/Screen Shot 2020-03-02 at 01.31.58.png>)

* Set a debug point on your AWS Lambda function.
* Now invoke your AWS Lambda function to hit on the debugging point.
* The session ends when your AWS Lambda function times out. You can change the timeout value of your function to perform longer debugging sessions.

### WebStorm

* Install [Thundra Debug Client](https://docs.thundra.io/ide-integrations/others#installation-of-thundra-debug-client).
* Select “`Run - Edit Configurations`” on WebStorm.
* Add a new configuration to “`Attach to Node.js/Chrome`”.

![](<../.gitbook/assets/Screen Shot 2020-03-03 at 17.14.18.png>)

* Enter a port number to which the Thundra Debug Client will listen.&#x20;
* Select “**`Chrome or Node.js > 6.3 started with --inspect”`** to attach to the configuration. Then click “**`OK`**”.

![](<../.gitbook/assets/Screen Shot 2020-03-03 at 17.17.36.png>)

{% hint style="info" %}
When starting the Thundra Debug Client, the port number configured here should also be given as either a command line argument or in the configuration file. For example, if you want to map the session name lab to the port 1111, the client should be started as:

`thundradebug -sp 1111=lab`
{% endhint %}

* Start thundra debug client provided with required parameters (auth token and port to session name mapping if not using default mapping `12358=default`) .

&#x20;       Example:

```bash
$ thundradebug --auth <your-auth-token> -sp 1111=lab 12358=default
```

* Set a debug point on your AWS Lambda function.
* Select the run profile created for Thundra from the Run Configurations drop-down menu.
* Click on the “Debug” button to start a debugging session with the selected profile.
* Now invoke your AWS Lambda function to hit on the debugging point.
*   The session ends when your AWS Lambda function times out. You can change the timeout value of your function to perform longer debugging sessions.&#x20;

