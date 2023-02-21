# VSCode Plugin

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

![Thundra Debugger on VSCode](https://github.com/thundra-io/thundra-vscode-issues/raw/master/resources/thundra-vscode.gif)

### Installation <a href="#installation" id="installation"></a>

* Install the extension from the [VSCode marketplace.](https://marketplace.visualstudio.com/items?itemName=thundra.thundra-debugger)
* Sign up for Thundra and select the Thundra Debugger option to receive your authentication key. If you’ve already signed up, you can get your key from the [Thundra AWS Lambda Debugger pane](https://start.thundra.io/apps#showDebugToken).
* Open the Command Palette (⇧⌘P) and select the `Thundra: Edit Configuration` command to modify the configuration file.

![Thundra Debugger Commanda Palette](<../../.gitbook/assets/image (44).png>)

* Set your authentication key to the variable “authToken” in your Thundra configuration file. [Learn how to get your authentication key.](../../getting-started/quick-start-guide/thundra-debugger.md#how-to-get-authentication-key-for-thundra-debugger)

{% code title="debug-client.json" %}
```javascript
{
    "profiles": {
        "default": {
            "debugger": {
                "authToken": <set-your-thundra-auth-token>,
                "sessionName": "default",
                "brokerHost": "debug.thundra.io",
                "brokerPort": 443
            }
        }
    }
}
```
{% endcode %}

### How to use

*   Set a debugging point on your AWS Lambda function.

    Execute the command `Thundra: Start Debugger` to start the debugging session. (Alternatively, you can click on the `Start Thundra Debugger` button on the Status Bar.)

![VSCode Status Bar](<../../.gitbook/assets/image (71).png>)

* Now invoke your AWS Lambda function to hit on the debugging point.
* The debugging session ends when your AWS Lambda function times out. You can update the timeout of your function for longer sessions.
* To manage your Thundra Debugger profiles:
  * Open the Thundra configuration file by executing the command `Thundra: Edit Configuration`.
  * Add new profiles to the file as shown below.

![Thundra Debugger Profile](<../../.gitbook/assets/image (78).png>)

* You can change your active profile by executing the command `Thundra: Change active profile` to use the Thundra Debugger with different configurations.

![Change Active Profile](<../../.gitbook/assets/image (60).png>)

In order to quickly start with a different session name:

* Execute the command `Thundra: Start Debugger (with a session name)`
* Enter a unique session name (this will not make changes to the configuration file).
* Invoke the Lambda function and start debugging.

### Commands

| Command                                         | Description                                                               |
| ----------------------------------------------- | ------------------------------------------------------------------------- |
| `Thundra: Start Debugger`                       | Starts the debugger with the current profile's settings.                  |
| `Thundra: Start Debugger (with a session name)` |  Starts the debugger with the given session name.                         |
| `Thundra: Change active profile`                |  Changes the active profile to the one given in your configuration file.  |
| `Thundra: Edit configuration`                   | Opens the file containing your debugger configuration so you can edit it. |

You can access all of the above commands from the command palette (Cmd+Shift+P or Ctrl+Shift+P).

{% hint style="warning" %}
When you start the Thundra Debugger in VSCode, if you are getting the _"Error processing attach: Error: Could not connect to debug target at ..."_ error; you can add `"debug.javascript.usePreview": false` configuration in your `settings.json` file. You can check out how to find `settings.json` file [here](https://code.visualstudio.com/docs/getstarted/settings#\_settings-file-locations).
{% endhint %}
