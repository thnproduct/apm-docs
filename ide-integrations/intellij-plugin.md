# IntelliJ IDEA Plugin

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

![Thundra Debugger on IntelliJ](https://github.com/thundra-io/thundra-intellij-issues/raw/master/resources/thundra-intellij.gif)

### Installation&#x20;

1. Install the extension from the [marketplace](https://plugins.jetbrains.com/plugin/13858-debugger-for-aws-lambda).&#x20;
2. Sign up for [Thundra](https://console.thundra.io/signup) and select the Thundra Debugger option to receive your authentication key. If you’ve already signed up, you can get your key from the Settings page. [Learn how to get your authentication key.](../getting-started/quick-start-guide/thundra-debugger.md#how-to-get-authentication-key-for-thundra-debugger)
3. Click on “`Run - Edit Configurations`” and then click the “Add” button to create a new configuration.
4. Select “`Thundra Debugger`” for the new configuration.
5.  Set your authentication key to the variable “Authentication Token” on the configuration modal. Enter a name for this new profile and then save.



![Thundra Debugger Configurations](<../.gitbook/assets/image (89).png>)

### How to use&#x20;

1. Set a debugging point on your AWS Lambda function.
2. Select a profile from the Run Configurations drop-down menu.
3. Click on the “Debug” button to start a debugging session with the selected profile.
4. Now invoke your AWS Lambda function to hit on the debugging point.
5. The session ends when your AWS Lambda function times out. You can change the timeout value of your function to perform longer debugging sessions.

#### &#x20;To manage your Thundra Debugger profiles:&#x20;

* Select “Run - Edit Configurations.”
* Select a profile from the Run Configurations drop-down menu.
* Click on “Thundra Debugger” and add a new configuration.
* Enter a new name for the profile and save it.
* Your saved profiles will be listed under “Run Configurations.”
