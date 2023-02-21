# Functions List Page

![Functions List Breakdown](<../../.gitbook/assets/image (280).png>)

The Functions List page can be considered Thundra Web Console's homepage, giving you a direct view of your functions that are being monitored. You may navigate to the page using the <img src="../../.gitbook/assets/image (303).png" alt="" data-size="original"> (functions) icon present in the navigation bar on the left side of the console.

The Functions List page displays all your deployed functions according to the query you set using the Query Bar and [Query Helper](../query-helper/query-helper-for-functions.md) at the top of the page. Moreover, you may also change the time constraint on the functions you are displaying using the “Time” option next to the Query Bar.

Overall, the Functions List page is the main page that enables you to navigate through all your deployed functions and see their monitoring data.

As can be seen from the illustration above, the Functions List page can be broken down into various components that help you filter through your functions and view all monitoring data pertaining to an individual function. These components are as listed below:

* [Query List](./#query-list)
* [Query Bar](./#query-bar)
* [Time Setting](./#time-setting)
* [Search Bar](./#search-bar)
* [Instrumentation](./#instrumentation)
* [Function Info](./#function-info)
* [Invocation Summary](./#invocation-summary)
* [Performance Summary](./#performance-summary)
* [Monitoring Options](./#monitoring-options)
* [List View](./#list-view)

### Instrumentation

If you connected Thundra to your CloudWatch logs via the CloudFormation stack, functions that are collected from CloudWatch can be easily instrumented from the Functions List page. We call this seamless monitoring. When you select functions, the number of instrumentable/uninstrumentable functions will be displayed in buttons, as shown below:

![](<../../.gitbook/assets/image (235).png>)

When the “Instrument” button is clicked, Lambda functions will be automatically instrumented and capabilities that are provided with instrumentation, such as distributed tracing and architecture, will be available.

Instrumentation from the console is available for the following runtimes:

* .NET
* Java
* Node.js
* Python

If a function is instrumented from the Thundra console, it can also be uninstrumented from the Thundra console.

You can get more information about seamless monitoring [here](../../monitoring/seamless-monitoring.md).

### Query List

The Query List component, which is on the left side of the Functions List page, lists all queries that you can use to filter the displayed functions. It is split into two sections: Predefined queries and Saved Searches.

The Predefined section lists queries that are created by Thundra to allow quick and convenient filtering of your Lambda functions. The following predefined queries are included:

* Most Costly - Order functions by monthly estimated cost in descending order
* Most Erroneous - Order functions by error count in descending order
* Most Invoked - Order functions by invocation count in descending order
* Node.js Functions - List only those functions whose runtimes are Node.js
* Slowest - Order functions by average duration in descending order
* Top Memory Consuming - Order by average memory usage in descending order
* Last Invoked - Order by last invocation time in descending order

The Saved Searches section of the Query List shows all the queries that you would like to save after entering them into the Query Bar or the [Query Helper](../query-helper/query-helper-for-functions.md).

To use a query in the Queries List, simply select a query and the function listed will be filtered and ordered depending on the selected query. Moreover, by clicking or hovering your mouse over a query, you will see two buttons appear to the right of the query, allowing you to either delete the selected query or set the query as the default query, and hence filtering your functions according to the default query whenever you visit your Thundra console.

### Query Bar

The Query Bar, which is located above the listed functions, allows you to write custom queries to filter through your functions. Three buttons are present at the end of the Query Bar, and they allow you to run, share, and save the written query respectively.

The Run button will execute whatever query is entered in the bar. The Save button will open up a dropdown menu, where you can save the query as either Query or Alert Policy. The Save Search Dialog will allow you to name the query you wish to save. The Save as Alert Policy view will allow you to save this condition as an alert policy. It is explained in detail [here](../alerts-page/creating-editing-alert-policies.md).

{% hint style="info" %}
#### Query Save Details

You can save your queries as public if your role is listed as Admin or Account Owner, so that everyone in the organization can see the saved query. You can also save your queries as private so that only you can see the saved query. If your role is listed as User or Developer, you can only save the query for yourself and your query won't be visible to the whole organization.
{% endhint %}

{% hint style="info" %}
#### Saving Queries as Alert Policies

If your role in the organization is `Admin`, `Account Owner,` or `Developer`, you can save queries as alert policies. The dropdown menu is not visible to the User role.
{% endhint %}

### Time Setting

The Time Setting dropdown menu is located to the right of the Query Bar, and you can use it to list all the functions that were last invoked within the chosen time setting. There are default time range settings for you to choose, but you may also specify your own custom range by clicking on "Custom Range," which will then bring up the Custom Range view. This view allows you to set the time range you would like to apply when viewing your Lambda functions.\


![](<../../.gitbook/assets/image (53).png>)

### Search Bar

The Search Bar allows you to see a list of your functions based on their names. The Search bar is located at the top of your functions list.

### List View

![](<../../.gitbook/assets/image (268).png>)



Three different list views are available to display different sizes of information:

* Comfort view
* Cozy view
* Compact view

![Comfort View](<../../.gitbook/assets/image (127).png>)

![Cozy View](<../../.gitbook/assets/image (218).png>)

![Compact View](<../../.gitbook/assets/image (281).png>)

### Function Info

Every listed function has basic information so that you can know which function you are looking at in the list. Function info is displayed in terms of:

* Function Name - The name of the Lambda function invoked
* Last Invoked Time - The time elapsed since the function was last invoked.
* Lambda Info - The runtime, AWS region, AWS Account Id, and project name of the Lambda function. The Lambda info is displayed in the form of various badges with different colors. The badges are arranged in the order listed below, with the following color codes:

![Function Info Legend](<../../.gitbook/assets/image (1) (1).png>)

###

### Invocation Summary

The Invocation Summary lists vital information of your Lambda function according to all its invocations. These statistics can allow you to get an overview of your Lambda function's performance, especially in terms of its erroneous and economic nature. The illustration below describes the statistic keys present in the view:

![Function Info Legend](<../../.gitbook/assets/function badge.png>)



It must be noted that “Estimated Cost”, “Estimated Monthly Cost,” and “Health” are calculated invocation stats whereas the other invocation stats are derived from the outcome performance of the invocation.&#x20;

* Health - the error count over the total invocation count
* Estimated Cost - AWS as the cost incurred from the invocation
* Estimated Monthly Cost - extrapolating the “Estimated Cost”
* Apdex score - If your configured your function for the apdex score, calculated score will be displayed here. For configuring your function visit documentation for [Node](../../node.js/nodejs-configuration-options/agent-configurations.md#configuring-apdex-score), [Python](../../python/configuration-options/agent-configurations.md#configuring-apdex-score) and [Java](../../java/configuration-options/agent-configurations.md#configuring-apdex-score).

### Performance Summary

An overview of the computing performance of each Lambda function can be viewed in the Performance Summary. The statistics provided aim to give you a clear picture of how your AWS Lambda is running concerning memory usage and duration, allowing you to tweak your functions accordingly. The statistics provided include average, median, and 99th percentile of runtime duration and memory usage.

### Monitoring Options

The monitoring options allow you to navigate to more detailed information on the function of choice. Clicking any one of these options navigates you away from the Functions List page to the Function Details page with the relevant information according to the data set selected from the Monitoring Options. This includes:

* [Invocations](../function-details-page/invocation-tab.md) - Lists all invocations of the specific Lambda functions, and allows you to see each invocation separately.
* [Performance Analysis](../function-details-page/performance-analysis-tab.md) - Allows you to analyze the overall performance of your Lambda function according to the performance of your invocation.
* [Metrics](../function-details-page/metrics-tab.md) - Provides graphical illustrations of the performance of the specific Lambda function.
