# Function Details Page

![Function Details Breakdown](<../../.gitbook/assets/image (234).png>)

The Function Details page provides detailed and insightful monitoring data of your function, and even allows you to navigate through monitoring data of specific invocations. You can access this page via the Functions List page. The details for a specific function can be viewed when you select specific monitoring data from the Monitoring Options, located on the right side of the [Functions List](../functions-list-page/) page. The Function Details page can also be displayed by clicking on a function name on the Functions List page.

This page contains two main sections. The first is the top section, which displays function stats and overall runtime figures. The second part of the page displays detailed monitoring data, which is split into four tab views.&#x20;

The Function Details page has three main components that allow you to navigate through the details of your selected function. These are listed as:

* [Function Profile](./#function-profile)
* [Monitoring Data Tabs](./#monitoring-data-tabs)

### Function Profile

![Function Profile Breakdown](<../../.gitbook/assets/image (184).png>)

The function profile section gives you detailed information about a specific function, including the function name, Lambda info, and function stats.

* Function Name - The name of the function being analyzed.
* Function Stats - Statistics related to all the invocations of the function.
* Lambda Info - This section is displayed in the form of various badges with different colors. The badges are arranged in the order listed below, with the following color codes.

![Function Info Legend](<../../.gitbook/assets/image (123).png>)

#### Custom Tags

![](<../../.gitbook/assets/image (11).png>)

In addition to Lambda information, any tags that users have defined will be displayed here. By clicking on a tag, you can filter functions that have this tag and value.\


![Functions List Page Filtered by Tags](<../../.gitbook/assets/image (95).png>)

### Monitoring Data Tabs

This section provides access to all monitoring data collected by Thundra. The tab views offer different views of monitored data, and you can switch among the tabs to get detailed and comprehensive monitoring data of not only your function as a whole but also of individual invocations. The followings tabs are included:

* [Invocations](invocation-tab.md) - Lists all the invocations within a specified time range, along with any filter queries specified within the tab view. Selecting any one of the invocations allows you to see its detailed trace and log information, which covers two of the three pillars of observability. Visit the [Invocations Tab](invocation-tab.md) view section to learn more about querying invocations and interacting with the list of invocations.
* [Performance Analysis](performance-analysis-tab.md) - Aims to give an overall insight into the state of individual invocations and how they affect the overall metrics. Thundra has implemented special statistical illustrations to make the information as accessible as possible. Visit the [Performance Analysis Tab](performance-analysis-tab.md) to learn more about the various graphical aids provided and how to read them to get a better insight into the effect of individual invocations on the overall status of your function.
* [Metrics](metrics-tab.md) - Illustrates various metric data in the form of detailed and comprehensible graphs. These graphs show all the invocations within a specified time period. The graphs presented in the Metrics tab are agent-specific, and each language agent has a specific manner of calculating the statistics displayed in the graphs. Visit the [Metrics Tab](metrics-tab.md) to get a detailed list of metric graphs according to the programming language, and to learn how to read the graphs.
