# Root Cause Analysis of Outliers

Serverless applications are the epitome of highly-distributed small black boxes. The nature of these applications makes it difficult to discover and troubleshoot problems for your functions. Thundra helps you monitor your applications by providing you details of each of your Lambda functions, allowing you to easily find problems and display invocation metrics. By using the guidance on our [Performance Analysis Page](../thundra-web-console/function-details-page/performance-analysis-tab.md), it becomes extremely easy to detect outlier invocations and check for downstream services in just two clicks of your mouse.

To analyze your monitoring data, you can navigate to the [Performance Analysis tab](https://console.thundra.io/dashboard) by selecting any function that you want to examine more closely to detect outliers from the [functions](../thundra-web-console/functions-list-page/) page.

The Performance Analysis tab provides intelligent and extremely valuable information about your Lambda function invocations. All the information is represented in the form of a heatmap and graphs that allow you to better comprehend and access the invocation data. From the performance analysis charts provided, you can easily detect problematic invocations, dive into their performance, and isolate the issue.

**Heatmap**&#x20;

On the Heatmap section, invocations for a function are plotted by time against duration. Depending on the interval of time and duration, the invocation is plotted as a square within that interval. As shown in the legend, a cell with a darker color indicates a higher invocation count within the specified interval.

When it comes to identifying outliers and analyzing their root cause, the selection mode of the heatmap is very helpful. Choose the selection mode and select an area with your cursor to find more insight about specific outliers. Charts, invocations, and resource usages are displayed under the heatmap and are modified when you select an area on the heatmap.

**Resource charts**

Services for a specific function and invocation can be seen at a glance with the usages on the Resource Chart section. Using a resource chart, you can see the breakdown of service usages and which services consume more time compared to the time overall.

In the Duration and Count chart, learning why a selected outlier occurred can be easily observed by simply looking at error and cold start counts or duration changes within a time range.

All the invocations in a selected area are displayed at the bottom. If you want to dive deeper into each invocation to analyze outliers, click on one of them. By doing this, you will be able to see trace data and logs, among other detailed information, pertaining to that specific invocation.

The image below shows how errors and resource usages are displayed for an outlier invocation.

![](<../.gitbook/assets/image (249).png>)

### How to configure your serverless system for metrics

You can easily use this performance summary for outlier detection and root cause analysis. Learn how to configure your system in each environment by visiting our documentation pages:  [Java](../java/configuration-options/metrics-configurations.md), [Python](../python/configuration-options/metric-configurations.md), [NodeJS](../node.js/nodejs-configuration-options/metric-configurations.md), .[NET](../.net/configuration-options/metric-configurations.md) and [Go](../go/go-configuration-options/metric-configurations.md).
