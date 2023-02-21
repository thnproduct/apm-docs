# Query Helpers for Requests

The Query Bar is available on the [Requests Tab](../api-details-page/requests-tab.md) of the [API Details](../api-details-page/) page at the top of the Requests List. When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.&#x20;

The Query Helper for requests has six different sections:&#x20;

* Request Filters - Filters API requests according to request properties.
* Statistical Filters - Filters API requests which display performance metrics according to the criteria you set.
* Sort Parameters - Sorts the list of API requests by the sorting criteria you set.
* Cheat Sheet - Provides example queries for your requests list.
* You can add new filters by clicking on the plus button in each row.

When you start to write your own query without using the Query Helper, the Query Helper will disappear and manual mode will be enabled. If you want to return to the Query Helper, just click on the "X" button.

### Request Filters

Request filters allow you to filter your API requests via request property parameters. For example, you could use the request specific parameters to obtain all requests that belong to a specific endpoint. The parameters available are explained in greater detail below.

**Endpoint**: Filters your API requests depending on their endpoint.

**Method**: Filters your API requests depending on their method type, which can be Query or Mutation for AppSync APIs and GET or POST for API-Gateway APIs.

**Erroneous**: Filters your API requests depending on whether or not they resulted in an error.

**Error Type**: Filters your API requests by the type of error that occurred.

### Statistical Filters

Statistical filters allow you to filter through your API requests using exact numerical values related to the various performances of each request. Unlike statistical filters in the Query Helper for functions, the statistical filters that you can set for requests are quite simple.&#x20;

![](<../../.gitbook/assets/image (83).png>)

Using statistical parameters in the query involves four fields that you can set:

* Operator - Operators are not applicable for request statistics. You need to select NONE.
* Metric - The Metric field will display the appropriate metric parameters to select from, which include both:
  * Duration: Execution time of the request.
  * Status: Status code of the request. You can use <>= operations for the status codes, such as requests that end with a status code that is over 400.
* Relation - After selecting the metric of your choice, you can choose the relation for the criteria to work on from the Relation field. <,>,>=,<=,=
* Value - Finally, fill in the value to filter by in the Value field. Click on the plus button to add more criteria.

### Sort Parameters

These parameters allow you to order your list of requests according to the parameters set. Similar to the statistical filters, setting the operator you would like to sort by changes the metrics that you can choose from, which are also similar to those available with the statistical filters option. The final field allows you to choose whether you would like to sort by ascending or descending order.

Click on the plus button to add more criteria.

### Cheat Sheet

If you have trouble while writing a query for your requests, you can take a look at the queries that we automatically prepare for you. Click on the "Go to Cheat Sheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your API requests using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (84).png>)
