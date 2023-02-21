# Query Helper for Traces

![](<../../.gitbook/assets/image (7).png>)

The Query Bar is located on the Trace List Tab at the top of the Traces List. When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for traces has four different sections:

* [Trace filters](query-helper-for-traces.md#trace-filters) - Filters traces according to specific properties for traces.
* [Statistical filters](query-helper-for-traces.md#statistical-filters) - Filters traces which display performance metrics according to the criteria you set.
* [Sort parameters](query-helper-for-traces.md#sorting-parameters) - Sorts the list of traces by the sorting criteria you set.
* [Cheat Sheet](query-helper-for-traces.md#cheat-sheet) - Provides example queries for your traces list.

### Trace Filters

Trace Filters allow you to filter your traces via specific property parameters. For example, you could use the trace specific parameters to obtain all traces with a specific error type. The parameters available are explained in greater detail below.

**Erroneous**: Filters your traces depending on whether or not they resulted in an error.

**HasError**: Filters your traces depending on whether or not they include an error.

**HasColdStart**: Filters your traces depending on whether or not they have a cold start.

**ErrorType**: Your traces could result in various types of errors. Hence, you can filter your traces by error-type, grouping traces depending on what error they resulted in.

### Statistical Filters

Statistical parameters allow you to filter through your traces using exact numerical values related to the various runtime performances of each trace. Unlike Statistical Filters in the Query Helper for Functions, the Statistical Filters that you can set for traces are quite simple. No operator is needed, and the Metric field lists only two options to choose from:

* Duration
* Start Time

![](<../../.gitbook/assets/image (111).png>)

Setting statistical parameters in the query involves 4 fields that you may set:

* Operator - Operators are not applicable for the traces statistics. You need to select `NONE`.
* Metric - The Metric field will display the appropriate metric parameters to select from.&#x20;
*   Relation - After selecting the metric of choice from the displayed list of metrics in the Metric filed, you can choose the relation for the criteria to work on from the Relation field.&#x20;

    `<,>,>=,<=,=`
* Value - Finally, fill in the value by which to filter in the Value field. After setting all the parameters in the respective fields. Click on the plus button to add more criteria.

### Sorting Parameters

These parameters allow you to order your list of traces according to the parameters set. Similar to the Statistical Filters, setting the operator you would like to sort by changes the metrics that you can choose from, which are also similar to those available when you use Statistical Filters. The final field allows you to choose whether you would like to sort by ascending or descending order.

* Start time
* Duration

Click plus button to add more criteria.

### Cheat Sheet

If you have trouble while writing a query for your traces, you can take a look at the queries that we automatically prepare for you. Click on the "Go to Cheat Sheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your traces using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (43).png>)

Below are more example queries for you.

To list traces that have at least one cold start invocation:

```
HasColdStart=true ORDER BY StartTime DESC
```

To list traces that have a duration of more than a second (1000ms):

```
Duration > 1000 ORDER BY StartTime DESC
```

To combine two (or more) conditions together, you can use the “AND” keyword. For example, the following query will return traces that have at least one erroneous invocation and that have a duration longer than 10 seconds (10000ms):

```
HasError=true 
AND Duration > 1000 
ORDER BY StartTime DESC
```
