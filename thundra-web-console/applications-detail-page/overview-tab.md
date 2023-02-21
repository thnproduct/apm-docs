# Overview Tab

The Overview tab allows you to see overall metrics of your application at a glance. In this tab, you can display endpoints of an application in a selected order and sort them with predefined sorting options.

![](<../../.gitbook/assets/image (51).png>)

You can navigate to the Overview tab by accessing the [Applications](../applications-page.md) page from the navigator bar on the left-hand side of the console. You can then select an application to analyze from the list displayed and go to the  [Application Details](../applications-page.md) page, where you can select Overview from the tab view.

The Overview tab consists of the following sections:

* Endpoints List
* Metrics

### Endpoints List

Endpoints of the selected applications are listed on the left-hand side of the tab, with a dropdown list available that includes sorting options that order endpoints according to a selected metric. The following metrics are used for sorting endpoints:

* Most Time-Consuming - Orders your endpoints by their time consumption considering overall latency, using the percentage of total latency consumed by an endpoint.
* Average Response Time - The average response time of an endpoint in terms of milliseconds.
* Most Erroneous - The percentage of erroneous transactions of an endpoint.
* Highest Throughput - The traffic of endpoints, in terms of request per second.

When you select an endpoint from this list, the right-hand side of the Overview tab will change and display the endpoint’s metrics.

### Metrics

On the right-hand side of the Overview tab, charts will be displayed that give you insights about your application. When this tab opens, the Metrics section displays an overview of the application. Using this section, you can select one of the endpoints from the left, which will then give you an overview of the endpoint.

The following charts and tables are available:

* Request and Error Count Chart - The request and error count for an endpoint or application within a selected time range. When you hover your mouse over the chart, you can display this count according to a specific time.
* Error Type Chart - Erroneous transactions can have different types of errors. This chart displays which error types occurred and their counts.
* Latency Chart - Average, P99, P90, and P50 latency change within a selected time range for an application or endpoint.
* Latency Distribution Chart - Transaction count distribution over latency.
* Latency Breakdown Table - The top five resources consuming time for an application or endpoint.
* Transactions Table - The five latest transactions for an endpoint or application. To display all transactions, click the "See all transactions" link.
