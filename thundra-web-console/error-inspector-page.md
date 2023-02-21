# Error Inspector Page

![](<../.gitbook/assets/image (106).png>)

Thundra offers you different ways to monitor your applications. You can be aware of the specific issues that you set alerts for. In addition to these alerting feature, you can have an insight into bottlenecks in you Lambda functions via Error Inspector Page.

When an error occurred in an AWS Lambda function, you can see this error-lambda pair on the error inspector list automatically. You don't need to set any configuration if you connect AWS account to Thundra. If you don't integrate Thundra with your AWS account, you can easily connect as explained [here](profile/settings-page/aws-tab/).

You can navigate to the Error Inpector page using the icon on the left-side bar.

![](<../.gitbook/assets/image (113).png>)

There are different actions and details for an error:&#x20;

* [Error Details](error-inspector-page.md#error-details)
* [Create/Edit Notification](error-inspector-page.md#create-edit-notification)
* [Enable/Disable Notification](error-inspector-page.md#enable-disable-notification)
* [Resolve an Error](error-inspector-page.md#resolve-an-error)

### Error Details

You can have more detailed information about an error from the error details. When you click on the error at the end of the line, error details are displayed.&#x20;

![](<../.gitbook/assets/image (66).png>)

There are different 3 different parts of an error detail:

* [Histogram](error-inspector-page.md#histogram)
* [Invocations](error-inspector-page.md#invocations)
* [Stack Trace](error-inspector-page.md#stack-trace)

#### Histogram

Histogram chart provides you the distribution of error's occurrence within the selected time range.&#x20;

Time intervals displayed on the histogram chart change according to the selected time period. The time intervals for a bar can be 1 min, 1 hour, or 1 day. When you hover on the chart, you can display the error count and time for a bar.

#### Invocations

You can display the last five invocations of the function includes the error. When you click on an invocation from this list, you will navigate to the [invocation detail page](invocation-detail-page/) to get more insight. If you want to see more invocations for this error-Lambda pair, click on the "See All Results" link. Then, you will display the [Invocations Tab](function-details-page/invocation-tab.md) on the [Function Details Page](function-details-page/) with the ErrorType parameter filled.

#### Stack Trace

On the right side of the error details, you can see the stack trace of the error. This stack trace belongs to the last invocation with this error.

### Create/Edit Notification

If you want to get notified when an error that is on the list occurred, you can create a notification from the Error Inspector page. Click on the "Create Notification" button on a row and select a notification preference from the opened modal.&#x20;

![](<../.gitbook/assets/image (48).png>)

You can edit your notification preferences if you already set them before. Hover your mouse above the notification icon on the error row and click on the "Edit" button. You can add/delete notification preferences and save it from here. If you want to delete notification completely, remove all preferences on the opened modal.

![](<../.gitbook/assets/image (41).png>)

If you don't set configurations for any alert integration you can set them on the [Alert Integrations page](profile/settings-page/alert-integrations-tab/) and display them during creating a notification.

There can be numerous events for an error but receiving alerts constantly can be annoying. We will throttle notifications for an error-lambda notification after ten alerts for one hour.

### Enable/Disable Notification

After setting an notification for an error-Lambda pair, the "Notifications Enabled" toggle for the row will be available. You can stop receiving notifications without deleting notification configuration by using this toggle. When you set a notification, this toggle will be opened automatically.&#x20;

### Resolve an Error

You can have an idea about an occurred error and its cause sometimes. When you are within the knowledge of an error, you don't want to see it on the list. You can resolve an error by clicking "Resolve" button on the row.

If this error is occured for that specific Lambda function again, it will be displayed on the list.
