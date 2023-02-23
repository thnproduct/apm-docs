# Query Helper for Invocations

![](<../../.gitbook/assets/image (77).png>)

The Query Bar is available on the Invocation Tab at the top of the Invocations List. When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for invocations has six different sections:

* [Invocation Filters](query-helper-for-functions.md#function-filters) - Filters Lambda invocations according to functional properties.
* [Custom Tags](query-helper-for-invocations.md#custom-tags) - Filters Lambda invocations according to tags that you defined.
* [Resource Tags ](query-helper-for-invocations.md#resource-tags)- Filters Lambda invocations according to tags for resources added by Thundra.
* [Statistical Filters](query-helper-for-invocations.md#statistical-filters) - Filters Lambda invocations which display performance metrics according to the criteria you set.
* [Sort Parameters](query-helper-for-invocations.md#sort-parameters) - Sorts the list of Lambda invocations according to sorting criteria you set.
* [Cheat Sheet](query-helper-for-invocations.md#cheat-sheet) - Provides example queries for your invocations list.

You can add new filters by clicking on the "+" button in each row.

When you start to write your own query without using the Query Helper, the Query Helper will disappear and manual mode will be enabled. If you want to return to the Query Helper, just click on the "X" button.

### Invocation Specific Parameters

Invocation Filters allow you to filter your Lambda invocations via invocation property parameters. For example, you could use the invocation specific parameters to obtain all invocations that resulted in a cold start or a specific error-type. The parameters available are explained in greater detail below.

**Cold Start**: Filters your Lambda invocations depending on whether or not they cause a cold start.

**Erroneous**: Filters your Lambda invocations depending on whether or not they resulted in an error.

**Timeout**: Filters your Lambda invocations depending on whether or not they resulted in a timeout.

**Error Type**: Your invocations could result in various types of errors. Hence, you can filter your Lambda invocations by error-type, grouping invocations depending on what error they resulted in.

### Custom Tags

You can configure your Lambda invocations to use custom tags, meaning that you can give business values as key:value pairs from a Lambda function and monitor them and their values from the Thundra console.

{% hint style="info" %}
To see how to use tags for the applications, look at this [blog](https://blog.thundra.io/enhancing-distributed-tracing-with-business-context).
{% endhint %}

You can filter your Lambda invocations using these custom tags, which are listed in the drop-down menu. Select one and enter the value that you want to filter.

You can set more than one value by using the comma separator.

```
tags.user.id = 1,2
```

### Resource Tags

These tags are predefined by Thundra to help you filter your invocations using resource data. These tags also work the same as Custom Tags do. You can select one of the tag keys and then enter the value you want to filter.

The query displayed below filters invocations that are triggered by AWS S3:

```
tags.trigger.className IN AWS-S3
```

### Statistical Filters

Statistical parameters allow you to filter through your Lambda invocations using exact numerical values related to the various runtime performances of each invocation. Unlike Statistical Filters in the Query Helper for Functions, the Statistical Filters that you can set for invocations are quite simple. No operator is needed, and the Metric field lists only two options to choose from:

* Duration
* Last Invocation Time

![](<../../.gitbook/assets/image (14) (1).png>)

Using statistical parameters in the query involves four fields that you can set:

* Operator - Operators are not applicable for invocation statistics. You need to select NONE.
* Metric - The Metric field will display the appropriate metric parameters to select from.
* Relation - After selecting the metric of your choice, you can choose the relation for the criteria to work on from the Relation field. `<,>,>=,<=,=`
* Value - Finally, fill in the value to filter by in the Value field. Click on the plus button to add more criteria.

### Sort Parameters

These parameters allow you to order your list of invocations according to the parameters set. Similar to the Statistical Filters, setting the operator you would like to sort by changes the metrics that you can choose from, which are also similar to those available when you use Statistical Filters. The final field allows you to choose whether you would like to sort by ascending or descending order.

Click on the plus button to add more criteria.

### Cheat Sheet

If you have trouble while writing a query for your invocations, you can take a look at the queries that we automatically prepare for you. Click on the "Go to Cheat Sheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your Lambda invocations using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (88).png>)

Below are more example queries for you.

To list the latest erroneous invocations:

```
Erroneous=true ORDER BY LastInvocationTime DESC 
```

To list applications with a specific error type:

```
ErrorType=DemoIllegalAccessException 
ORDER BY LastInvocationTime DESC
```

To list applications with business tags that you can set for your functions: _(serviceName is used as a custom tag here)_

```
appTags.serviceName IN (user,team) 
ORDER BY LastInvocationTime DESC
```

{% hint style="info" %}
To see how to use tags for the invocations, look at this [blog](https://blog.thundra.io/enhancing-distributed-tracing-with-business-context).&#x20;
{% endhint %}

To combine two (or more) conditions together, you can use the “AND” keyword. For example, the following query will return invocations that have the string tag of "user.id" and that have a duration lasting longer than a second (1000 ms):

```
tags.user.id="1" AND 
Duration > 1000 
ORDER BY LastInvocationTime DESC
```

You can sort invocations according to various fields, such as duration and last invocation time. In the following example, you will run the previous query but sort the invocations according to their duration from longest to the shortest:

```
tags.user.id="1" AND 
Duration > 1000 
ORDER BY Duration DESC
```
