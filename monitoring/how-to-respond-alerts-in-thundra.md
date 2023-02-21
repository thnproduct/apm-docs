# How to Respond Alerts in Thundra

Monitoring and troubleshooting your serverless system is crucial. You need to be able to take action as fast as possible when problems and errors occur in order to maintain your system’s stability. Thanks to Thundra’s alerting feature, you can easily [stay aware of issues ](staying-aware-of-issues-with-alerts.md)in your serverless system. The alerting capability sends you notifications for a predefined condition, and you can quickly solve issues as soon as they occur.

You can configure alerts for invocations, functions, and traces using Thundra's detailed queries. Defining a problematic occurrence and creating an alert for it is just part of handling issues, however. The most important part of the process begins after you receive an alert notification: finding the problem that caused the alert and then resolving it. Thundra helps you find the problem with just one click of your mouse!

![Alert Mail](<../.gitbook/assets/image (297).png>)

When an alert occurs, a message will be sent to the notification channel you selected (email or Slack). Click on the link inside the message to see details of the alert event. You can also go directly go to the results of the event by clicking on the “See Results” link.

![Alerts Page](<../.gitbook/assets/image (131).png>)

If you chose not to be notified via message,  you will see a number listed for unresolved alerts on the Alerts page icon. To see more about these alerts, you can open the Alerts page and then click on the “Display Events” button of a problematic alert policy. This will open the [Events](../thundra-web-console/alerts-page/events-tab.md) page with a specific alert filter; you can click on any event to see its details.

![Event Detail](<../.gitbook/assets/image (166).png>)

After you select the event that caused the alert, you will see an overview of the situation. You can also click on the "See all results" option to learn more about the invocation, function, or trace that triggered the alert. While this option offers more in-depth information, you can still easily understand the issue with just a glance.

The “See all results” button will navigate you to the [Invocations](../thundra-web-console/function-details-page/invocation-tab.md), [Functions](../thundra-web-console/functions-list-page/) or [Traces](../thundra-web-console/traces-page.md) pages based on the type of event that occurred. On this page, items will be listed in the time range that the event occurred in. If you want to see what happened in the other parts of your system during the alert, you can click the button in the notification to set the global time interval to the time when the alert occurred.

At the bottom of the page, Resource Usages and Statistics are displayed to understand what happened in the function, invocation, or trace. These metrics give you more insight about your system’s behavior during the event.

Thundra’s alerts were designed to notify you about bottlenecks in your serverless system as early as possible while also reducing the MTTR even more by helping you quickly understand what goes wrong in your system.

### How to handle alerts in Thundra?

You can visit our documentation for [creating](../thundra-web-console/alerts-page/creating-editing-alert-policies.md) and [handling](how-to-respond-alerts-in-thundra.md) alerts.
