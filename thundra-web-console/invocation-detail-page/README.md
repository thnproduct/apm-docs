# Invocation Detail Page

![Invocation Detail Breakdown](<../../.gitbook/assets/invocation detail.png>)

The Invocation Detail page displays trace and log data of a specific invocation, along with other metrics, giving you an overview of how your Lambda function performed during a specific invocation. This page consists of two sections: the **** [Invocation Profile](./#invocation-profile) and [Monitoring Data Tabs](./#monitoring-data-tabs).

You can display the Invocation Detail page by first selecting any function from the [Functions List](../functions-list-page/) page, and then by selecting a specific invocation from the [Invocations Tab](../function-details-page/invocation-tab.md) on [Function Details](../function-details-page/) page. &#x20;

### Invocation Profile

The Invocation Profile section displays all related information and statistics about a specific invocation.

![Invocation Profile Breakdown](<../../.gitbook/assets/image (109).png>)

Three fields are included in the Invocation Profile:

* **Function name**: Name of the Lambda function with a trace badge of when the invocation occurred and where it is located in which traces. You can click and display the Trace Map to show where the function is located.
* **Invocation stats:** Invocation statistics along with the exact time of the invocation, and the request ID to better identify the invocation.
* **Invocation Info:** Displays badges. The first three indicate the runtime, region and AWS account, and profile information of the invocation. The last two indicate whether an error has occurred, the type of error, and whether or not the invocation resulted in a cold start. If there is an error in an invocation, hover your mouse over the error badge to see the stack trace.&#x20;

{% hint style="info" %}
You can set error and stack trace for an invocation using [manual instrumentation](../../getting-started/quick-start-guide/instrument-to-achieve-deeper-level-of-visibility.md#manual-instrumentation) o display it in the invocation info. However, if there is an error in the root span, this error will be displayed instead of the error you set for the invocation in the error badge.
{% endhint %}

**Custom Tags**

![](<../../.gitbook/assets/image (114).png>)

In addition to Invocation profile, if any tags that users defined, will be displayed here. By clicking on a tag, you can filter your invocations that have this tag and value.

![](<../../.gitbook/assets/image (103).png>)

### Monitoring Data Tabs

* [Trace Chart ](trace-chart-tab.md)- Illustrates the trace data collected with the invocation and displays the data in terms of traces and spans. These spans relate to every single interaction that the Lambda function had with external services, such as DynamoDB and Redis, along with all the functions that have been called upon by the function you selected, or by any other resources in the trace. Moreover, you can click on a span and get additional information, such as request and response data, tags embedded into the span, and span logs. Visit the [Trace Chart](trace-chart-tab.md) tab section to get further insight into how your trace data is displayed.
* [Logs](logs-tab.md) - Displays all the log data and console prints that your Lambda function creates according to the specific invocation you are analyzing. Logging is runtime specific; however, Thundra displays your logs irrespective of runtime in a uniform format, so that you can see uniform logs across several Lambda functions. Visit the [Logs](https://docs.thundra.io/docs/logs-tab) ttab section to get a better understanding of how your Log and console data is presented to you.
