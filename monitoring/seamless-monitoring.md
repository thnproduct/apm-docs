# Seamless Monitoring

CloudWatch integration with Thundra makes it easy for new users to adapt to Thundra and monitor their functions. Within minutes, Thundra retrieves all  the functions in your AWS account. However, core capabilities of Thundra, such as distributed tracing, application architecture, and performance insights, will not be available without instrumenting functions. Lambda functions can be instrumented by adding Layer, setting up the API key, and configuring Thundra’s libraries. But Thundra provides you with an easier way to instrument your functions: Seamless Monitoring.

![Function Selection](<../.gitbook/assets/image (226).png>)

On the [functions list](../thundra-web-console/functions-list-page/) page, you can display all functions that were gathered from CloudWatch after CloudWatch integration or manual instrumentation. Simply select the functions you want to instrument and click on the “Instrument” button. You can instrument a function from the Thundra console if:

* It has the following runtimes:
  * Node.js
  * Java
  * Python
  * .NET
* It is integrated with CloudWatch or manually instrumented.

![](<../.gitbook/assets/image (163).png>)

A few moments after you click on the “Instrument” button, your functions will be ready. After this, you can display the architecture, distributed tracing, and performance analysis features of Thundra. The Thundra icon shows you in just a glance whether or not your function is manually instrumented.

![](<../.gitbook/assets/image (172).png>)



For Lambda functions with Java, an additional feature is provided: Fast Start-up. Fast startup allows you to have a lower cold start overhead with Thundra's “Custom Runtime” support and can be found as an additional option under the “Instrument” button when any Java function is selected. Select a Java function from the functions list, click "Instrument with Fast Start-up," and with just that one step, you gain an unmatched initialization time for your Java functions.

Functions that were instrumented from the Thundra console can also be un-instrumented by simply selecting a function and clicking the “Un-instrument” button.&#x20;
