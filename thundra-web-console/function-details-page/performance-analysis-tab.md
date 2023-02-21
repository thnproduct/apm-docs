# Performance Analysis Tab

![Performance Analysis Breakdown](<../../.gitbook/assets/image (215).png>)

The Performance Analysis tab provides intelligent and extremely valuable information about your Lambda function invocations. All the information is represented in the form of charts and graphs to allow you to better comprehend and access the invocation data. Using the Performance Analysis charts, you can easily detect problematic invocations, dive into the details of their performance, and isolate the issue.

You can display the Performance Analysis tab by first navigating to the Functions List page, and then selecting any function to analyze. From the Function Details page, you will be redirected to the Performance Analysis tab.

![Performance Analysis Tab View](<../../.gitbook/assets/image (279).png>)

The Performance Analysis tab consists of three main data illustrations that visualize information regarding your invocations. The following items are displayed in this tab:

* Heat Map
* Resource Usage Chart
* Duration and Count Chart
* Invocations Table

### Heat Map

![Heat Map Breakdown](https://files.readme.io/7037dc3-heat\_map\_breakdown.png)

The Heat Map is the main component of the Performance Analysis tab. It graphically illustrates all your invocations within a selected time period using the Time Setting, which is located on the top right side of the[ Function Details](https://docs.thundra.io/thundra-web-console/function-details-page) page.

The Heat Map plots invocations according to the time and duration of the invocation execution. The plot has time intervals on the x-axis, and duration time intervals on the y-axis in ms. Depending on the interval the invocation falls in, it is plotted accordingly on the respective interval square on the Heat Map. The Legend can be used to understand how many invocations are within a square on the plot, with darker colors indicating a higher number of invocations lying within that interval square. In the illustration above, the Legend indicates that the highest number of invocations that are present in any square is 4; similarly, the least number of invocations in any square is 0.

Moreover, the Heat Map has two views, which you can toggle between using the View Selection. The two views in the View Selection are:

* Histogram - Plots a histogram chart of all the invocations that lie within a certain column, as indicated by the prompt. This allows you to see the count of invocations against duration intervals within the histogram.
* Selection - Allows you to select a group of invocations. This, in turn, affects the data shown on the[ Resource Usage](https://docs.thundra.io/thundra-web-console/function-details-page/performance-analysis-tab#resource-usage-chart) Chart,[ Duration and Count Chart](https://docs.thundra.io/docs/performance-analysis#section-duration-and-count-graph), and the Invocation Table, which only displays those invocations within the selection. Hence, you can select any invocation from the Heat Map that you think is problematic, and dive further into those invocations with the other analysis tools.

![Heat Map Selection View](https://files.readme.io/aa41968-heat\_map\_selection.gif)

### Resource Usage Chart

The Resource Usage Chart lists all the services of the invocations, along with how many resources each service took overall. Thundra calculates this by comparing how much each service took in terms of time. The data used encompasses all the invocations present on the[ Heat Map](https://docs.thundra.io/thundra-web-console/function-details-page/performance-analysis-tab#heat-map) plot, or in the selected region of the[ Heat Map](https://docs.thundra.io/thundra-web-console/function-details-page/performance-analysis-tab#heat-map). At the end of the process, the load of running each service is represented in percentages. You can see from the illustration above that writing to PostgreSQL took up the most time. This allows you to see which of your services in your Lambda function are the most resource-intensive.

![Resource Usages of Services](<../../.gitbook/assets/image (158).png>)

### Duration and Count Graph

![Duration and Count Chart](<../../.gitbook/assets/image (202).png>)

In the Duration and Count Graph, invocation counts and duration are visualized via bar and line charts.

The bar chart illustrates invocation counts in terms of erroneous and healthy cold start invocations within a specific time interval. The y-axis of the graph on the left shows these counts.

The line chart plots overall duration of the invocations throughout these time intervals. The y-axis of the graph on the right shows durations. There are three lines which indicate:

* Average durations
* Duration of the 99th percentile
* Duration of the 95th percentile

### Invocation Table

Unlike the[ Invocation Table](https://docs.thundra.io/thundra-web-console/function-details-page/invocation-tab#invocations-table) present in the[ Invocation](https://docs.thundra.io/thundra-web-console/function-details-page/invocation-tab) tab of the[ Function Details](https://docs.thundra.io/thundra-web-console/function-details-page) page, the Invocation Table displayed in the Performance Analysis tab only lists those invocations that are present on the[ Heat Map](https://docs.thundra.io/thundra-web-console/function-details-page/performance-analysis-tab#heat-map), or are that are within the selection region of the Heat Map plot. The table has the following fields:

* Trigger - Indicates what triggered the function to be executed (i.e., another function, DynamoDB, or any other trigger).
* Invocation Time - The time the function was invoked and the specific invocation that was received.
* Duration - How long it took for the Lambda function to be executed, in ms.
* Error - Whether or not there was an error in executing the function, and if so, what kind of error it was.
* Cold Start - Whether or not the specific invocation resulted in a cold start.
* Timeout - Whether or not the invocation resulted in a time out.
* Latency Breakdown - This is one of Thundra’s statistical illustrations to allow you to get an insight into how your Lambda function behaved in that specific invocation. The Latency Breakdown bar represents your total invocation, and the subparts of the bar represent the various services and functions that were executed and that interacted with each other during the invocation. Hence, you can see which integration or part of your Lambda function actually takes up which part of the invocation.

![](https://files.readme.io/b6f4a99-latency\_breakdown.gif)
