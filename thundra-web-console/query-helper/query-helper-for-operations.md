# Query Helper for Operations

![](<../../.gitbook/assets/image (63).png>)

When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for operations has four different sections:

* [Operation Filters](query-helper-for-operations.md#operation-filters) - Filters operations according to specific properties of operations.
* [Statistical Filters ](query-helper-for-operations.md#statistical-filters)- Filters operations which display performance metrics according to the criteria you set.
* [Sort Parameters](query-helper-for-operations.md#sorting-parameters) - Sorts the list of operations by the sorting criteria you set.
* [Cheat Sheet](query-helper-for-operations.md#cheat-sheet) - Provides example queries for your operations.

### Operation Filters

Operation Filters allow you to filter your operations via operation specific property parameters. For example, you could use the operation specific parameters to obtain all the operations with a specific runtime or from a specific project. The parameters available are explained in greater detail below.



Function Name: Filters your operations by name.

**Project**: Considering that your operations are grouped by project, you can filter them by a specific project or a collection of projects; the drop-down menu allows you to select several options. Depending on your selections, the values of the Stage parameter are also affected.

**Runtimes**: Refers to the AWS runtimes that are supported by Thundra. You can filter them by a specific runtime or a collection of runtimes, as the drop-down menu allows you to select several options.

**Resource Name**: Refers to the names of services that are supported by Thundra. You can filter your operations by the names of specific resources, which are listed in the drop-down menu,  and you have the ability to select several options.

**Resource Types**: Refers to types of services that are supported by Thundra. You can filter your operations by specific resources, which are listed in the drop-down menu, and you have the ability to select several options.

**Erroneous**: Filters your operations depending on whether or not they resulted in an error.

**Error Type**: Your operations could result in various types of errors. Hence, you can filter your traces by error-type, grouping operations depending on what error they resulted in.

### Statistical Filters

Statistical Filters allow you to filter through your operations using exact numerical values related to the various runtime performances of each operation. Unlike Statistical Filters in the Query Helper for Functions, the Statistical Filters that you can set for operations are quite simple. No operator is needed, and the Metric field lists only one option to select:

* Duration

![](<../../.gitbook/assets/image (14).png>)

Using statistical filters in the query involves four fields that you can set:

* Operator - Operators are not applicable for operation statistics. You need to select NONE.
* Metric - The Metric field will display the appropriate metric parameters to select from.
* Relation - After selecting the metric of your choice, you can choose the relation for the criteria to work on from the Relation field. `<,>,>=,<=,=`
* Value - Finally, fill in the value to filter by in the Value field.&#x20;

Click on the plus button to add more criteria.

### Sorting Parameters

These parameters allow you to order your list of operations according to the parameters set. Similar to the Statistical Filters, setting the operator you would like to sort by changes the metrics that you can choose from,which are also similar to those available when you use Statistical Filters. The final field allows you to choose whether you would like to sort by ascending or descending order.

* Start Time
* Durations

Click on the plus button to add more criteria.

### Cheat Sheet

If you have trouble while writing a query for your operations, you can take a look at the queries that we automatically prepare for you. Click on the "Go to CheatSheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your operations using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (5).png>)

Below are more example queries for you.



In order to list erroneous operations

```
Erroneous=true ORDER BY StartTime DESC
```

In order to list operations with a duration longer than a second:

```
 Duration > 100 ORDER BY StartTime DESC
```

In order to combine two or more conditions together, you can use AND keyword. For example; the following query will return the DynamoDB operations whose duration is longer than 1000ms.&#x20;

```
ResourceType=AWS-DynamoDB 
AND 
Duration > 1000 ORDER BY StartTime DESC
```
