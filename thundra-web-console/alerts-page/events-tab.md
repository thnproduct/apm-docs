# Events Tab

![Events Tab](<../../.gitbook/assets/image (251).png>)

The Events tab lists all events that violate your alert policies, starting from the most recent and descending to the oldest. You can display details of any event on the Event Detail page by clicking on an event in the list.

You can navigate to the Events tab by first navigating to the Alerts Page on the left-hand side bar, and then by selecting the Events tab on that page.&#x20;

![Events tab selected](<../../.gitbook/assets/image (141).png>)

You can display the list of events in terms of:

* Date/Time - Time when the violation caused the event.
* Alert policy - The violated alert policy for the event.
* Source - The function or functions that are violating the alert policy. Note that there can only be one function for the alert policies created from invocation queries. On the other hand, there can be more than one function for the function queries.
* Notification Status - This column shows whether the notification of the event has been sent correctly. When this column is not green, you can hover your mouse over this column and see the channels which couldn't receive the notification.
* Severity - The severity of the event inherited from its alert policy. Severity could be listed as `INFO`, `WARNING`, or `CRITICAL`.

### Event Details Page

![Event Details Page Breakdown](<../../.gitbook/assets/image (201).png>)

In the Event Details page, you can display violated event information listed in three sections:

* **Event Metadata** - The main data about a specific event. You can display the main properties of an event, including alert name, function name, notification type, severity, and accruing time.
* **Event Details** - Explains why this alert was actually created by showing the query, its schedule, and the time range in which the violating events occurred. You can see if you slightly over the threshold or well beyond it in the Condition section. Lastly, this section shows you the sample functions or invocations that caused the event. By clicking on See all results, you will be navigated to the respective function or invocation list (which is explained here).
* **Event Statistics** - Gives insights about the event. This section is displayed for events caused by queries filtering invocations, and is only available for invocation and function alert events.
  * _**Table**_ -  You are able to see the general metrics of your function in the time of the alert event and non-alerting times. The Baseline column shows the situation of the metrics during the time range neighboring the time range of the violation. In the Alert column, you can see the metrics from the time of the alert event.
  * _**Resource Usage**_ - You are able to display resource usages of your function in both the alerting and non-alerting times. The dotted line represents the baseline while the bar itself represents the alert event. For the example above, PostgreSQL usage decreased while SQS usage greatly increased during the time of the alert event.

### Seeing the violation events&#x20;

![](<../../.gitbook/assets/image (180).png>)

Clicking on `See all results` in the Event Detail page navigates you to the following listing pages:

* Functions List
* Invocations List
* Traces List
* Operations List

When you first land on this page, you will see a notice at the top of the page that says: "We set the time interval to `<startTime> - <endTime>` to list the violating events temporarily on this page. If you want to use this interval globally in the application, click here". This means that the Thundra console shows violating functions or invocations on this page without modifying the global time. If you want to check what actually happened in the other parts of your system during the time of the alert, you can click the button in the notification and permanently set the time interval of the alert.

