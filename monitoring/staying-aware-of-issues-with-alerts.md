# Staying Aware of Issues with Alerts

Thundra's search mechanism for functions, invocations, or traces allows you to filter them with detailed queries. To find problematic instances or detect if something goes wrong, you can write a specific query, such as:

```
AVG(Duration)>200 AND COUNT(ColdStart)>0
```

This query lists functions that have an average invocation time of more than 200 milliseconds and that also contain cold starts, all displayed in an easy-to-view list.  Searching for issues is a way to track your serverless system, but it’s even easier if you can check for these issues periodically using detailed queries. Thundra provides you the ability to troubleshoot problems in your serverless system and handle them by using **Alerts**.

![](<../.gitbook/assets/alert policy (1).gif>)

In Thundra, you can save queries that are written for functions, invocations, and traces as an alert policy, which will notify you about your system metrics and bottlenecks.

For example, if you would like to be notified when there are functions with health under 95 in one hour, all you have to do is write a query on the Functions Page and save it as an alert. While saving your alert policy, you can decide in which periods the condition should be checked, from "Every 1 minute" to "Every month."

You can also save several alert policies, which means you should give each on a name you will remember in order to recognize them later.  Alerts can also be defined with different levels of severity. “`INFO`” can be used to alert every user, “`WARNING`” can be used to notify you that something is wrong and needs to be fixed, and “`CRITICAL`” can be used to alert you to an extremely urgent problem.

You can be notified about alert policies in several different ways, including through email and Slack, or you can integrate Thundra alerts with OpsGenie, PagerDuty, and VictorOps. Should you become overwhelmed with receiving&#x20;

* **Throttle** -  After you get the first alert, it will be throttled. This solution is useful if you know that your system will violate this alert policy after the first time. Alerts can be throttled for minutes, hours, or days.
* **Disable** - If you don’t want to see an alert at all, you can disable it from theWhen your alert policy condition is met, you will be notified through your preferred notification channel. After this point, you can manage your alerts as detailed [here](how-to-respond-alerts-in-thundra.md).&#x20;

### Invocation Alerts

![Create Invocation Alert Policy](<../.gitbook/assets/image (288).png>)

Alerts for invocations and traces are slightly different compared to other alert policies. As with the other policies, you can write a detailed query and save it as an alert. While saving an alert for any invocation or trace, select a **target**. For invocations, this allows you to target not only the function you are working on, but also the other functions in the account or a specific project. Your target type selections allow you to check the condition of a single function, all functions in a specific project, or all functions in Thundra. For trace alerts, selecting a target allows you to set alerts for all the traces that occurred in your application or for unique traces that you selected.

### How to Set Alerts in Thundra

You can visit our documentation for [creating](../thundra-web-console/alerts-page/creating-editing-alert-policies.md) and [handling](how-to-respond-alerts-in-thundra.md) alerts for more information.



