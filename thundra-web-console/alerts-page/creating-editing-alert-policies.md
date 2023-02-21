# Creating/Editing Alert Policies

Thundra provides you with the capability to filter your functions, invocations, and traces with detailed queries. For example, you can filter out functions that have a specific tag,  that have an estimated cost above a certain threshold, and that have a number of cold starts below a certain threshold. This query may result in some functions, which will be listed in the Functions List.

If you want to save this query and periodically check its results, you can save your query as an alert policy. This feature is available for functions, invocations, and traces.

To save a query as an alert policy, click on the save button next to the Query Bar and select “Alert Policy.” Thundra will then open a new dialog for you to let you tune your alert policy.

![Save as Alert Policy Dialog](<../../.gitbook/assets/image (195).png>)

Alert policies have three different parts for saving/editing in common:

* **Alert Metadata** - With Alert Metadata, you set a name and severity type for your alert policy. A name is required for alert policies; this field cannot be blank. There are three different severity options that you can choose according to the importance level of your alert policy: INFO, WARNING, or CRITICAL. For example, you can set a CRITICAL level policy if you think that this policy is checking something crucial for your system. Similarly, you can just set it as INFO if you think that you just need to inform your colleagues about the results.
* **Condition** - This is the most important part of alert policies because it allows you to display the query that you write in the Query Bar.
  * _Schedule_ - You can set a periodic time for when your alert policy will be checked.
  * _Trigger_ - Your query may return thousands of results. You can select when Thundra should trigger your alert policy based on a specific number of results.
  * _Throttle_ - If you want to prevent new events after a condition has been satisfied, you can use the throttle threshold value. You can set a threshold time to protect your team from alert fatigue while they are troubleshooting the issue.
* **Notification** - If you want to be notified when an alert event has occurred, you can select a notification option. You are able to select one or more notification channels from the available options, including Slack, OpsGenie, Email, and Webhook.

If you create an alert policy without setting up a notification channel, you won't be notified. However, you can still display events that occurred from your alert policy on the Alerts page.

{% hint style="info" %}
If you create an alert policy without notification channel, you won't be notified. However, you can display events that is occurent from your alert policy in [Alerts](./) page.
{% endhint %}

## What's different in Alert Policies created from invocation queries?

When you are filtering the invocation of a function in Thundra, you can filter them as follows: tags.user.id="1" AND Duration>200 AND ColdStart=false. In this query, you filtered the invocations in which the function is processing the information of the user with id 1, the invocation duration is bigger than 200ms, and where there is no cold start. Like alert policies created from function queries, you can save these queries as alert policies.

![Target Selection](<../../.gitbook/assets/image (198).png>)

While saving invocation queries as alert policies, you will select the target type. You can prefer to target not only the function you are working on, but also other functions in the account or in a specific project. For example, you may want to check the same condition of every function dealing with users.

Using this section, you can decide whether your function checks the condition for only one function or for more than one function, for the functions of a specific project(s), or for all functions in your Thundra account.



