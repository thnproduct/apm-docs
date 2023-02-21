# Query Helper for Functions

![](<../../.gitbook/assets/image (23).png>)

When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for functions has five different sections:

* ****[**Function Filters**](query-helper-for-functions.md#function-filters) - Filters Lambda functions according to specific properties of functions
* ****[**Custom Tags**](query-helper-for-functions.md#custom-tags) - Filters Lambda functions according to tags that you have defined
* ​[**Statistical Filters**](query-helper-for-functions.md#statistical-filters) - Filters Lambda functions which display performance metrics according to the criteria you set
* ​[**Sort Parameters**](query-helper-for-functions.md#sort-parameters) - Sorts the list of Lambda functions according to the sorting criteria you set
* ​[**CheatSheet**](query-helper-for-functions.md#cheat-sheet) - Provides examples of queries for your functions list.

You can add new filters by clicking on the "+" button in each row.

![](<../../.gitbook/assets/image (15).png>)

While you start to write your own query without using the Query Helper, the Query Helper will disappear and manual mode will be enabled. If you want to return to the Query Helper, just click on the "X" button.

### Function Filters

Function Filters allow you to filter your Lambda functions via Lambda property parameters. For example, you could use the specific parameters for functions to obtain all the functions with a specific runtime or from a specific project. The parameters available are explained in greater detail below.

![](<../../.gitbook/assets/image (105).png>)

**Name:** Filters your Lambda functions by name.

**Project:** Considering that your Lambda functions are grouped by project, you can filter them by a specific project or a collection of projects; the drop-down menu allows you to select several options.

**Regions:** Refers to the AWS region where your Lambda function resides, which includes all the AWS regions commercially available for AWS Lambda. You can find a full list of regions available for AWS Lambda here. You can filter them by a specific region or a collection of regions, as the drop-down menu allows you to select several options.

**Runtimes:** Refers to the AWS runtimes that are supported by Thundra. You can filter them by a specific runtime or a collection of runtimes, as the drop-down menu allows you to select several options.

**AWS Account Number:** Refers to the AWS account number in which the Lambda function is included. You can filter your Lambda functions according to which AWS account they belong to by selecting the available account number from the list.

### Custom Tags

You can configure your application to use custom tags, meaning that you can give business values as key:value pairs from a Lambda function and monitor them and their values from the Thundra console.

{% hint style="info" %}
To see how to use tags for applications, look at this [blog](https://blog.thundra.io/enhancing-distributed-tracing-with-business-context).
{% endhint %}

You can filter your Lambda functions using these custom tags, which are listed in the drop-down menu. Select one and enter the value that you want to filter.

You can set more than one value by using the comma separator.

![](<../../.gitbook/assets/image (110).png>)

```
appTags.serviceName=user, team
```

### Statistical Filters

Statistical parameters allow you to filter through your Lambda functions using exact numerical values related to the various runtime performances of each function. These runtime performances include the following:

* Monthly Estimate Cost
* Estimated Cost
* Invocation
* Error count
* Timeout
* Cold Start count
* Duration
* Used Memory
* Allocated Memory

![](<../../.gitbook/assets/image (79).png>)

Using statistical parameters in the query involves four fields that you can set:

* **Operator** - First choose the operator type from the Operator drop-down menu: AVG, SUM, COUNT, MIN, MAX, PERC50/90/95/99.
* **Metric** - According to the operator you choose, the Metric field will display the appropriate metric parameters to select from.
* **Relation** - After selecting the metric of your choice, you can choose the relation for the criteria to work on from the Relation field. `<,>,>=,<=,=`
* **Value** - Finally, fill in the value to filter by in the Value field. Click on the plus button to add more criteria.



### Sort Parameters

This option allows you to order your list of functions according to the parameters you set. Similar to the Statistical Filters, setting the operator you would like to sort by changes the metrics that you can choose from, which are also similar to those available when you use Statistical Filters. The final field allows you to choose whether you would like to sort by ascending or descending order.

Click on the plus button to add more criteria.

### Cheat Sheet

If you have trouble writing a query for your functions, you can take a look at the queries that we automatically prepare for you. Click on the "Go to CheatSheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your Lambda functions using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (115).png>)

Below are more example queries for you.

To list applications in a specific region:

```
Region=eu-west-1 ORDER BY LastInvocationTime 
```

To list applications with more than a threshold number of timeouts:

```
COUNT(Timeout) >=10 ORDER BY LastInvocationTime DESC
```

To bring applications with custom tags that you set for your functions: _(serviceName is used as a custom tag here)_

```
appTags.serviceName IN (user,team) 
ORDER BY LastInvocationTime DESC
```

{% hint style="info" %}
To learn how to use tags for applications, look at this [blog](https://blog.thundra.io/enhancing-distributed-tracing-with-business-context).&#x20;
{% endhint %}

To combine two (or more) conditions together, you can use the “AND” keyword. For example, the following query will return applications that have the string tag of "user" and that have healthy (normal-erroneous/normal \* 100) invocations that are less than 95 percent of the total:

```
appTags.serviceName=user AND 
Health < 95 
ORDER BY LastInvocationTime DESC
```

You can sort applications according to various fields such as health, last invocation time, number of errors or invocations, and even cost. In the following example, you will run the previous query but sort applications according to the cost in a set time period:

```
appTags.serviceName=user AND 
Health < 95 
ORDER BY EstimatedCost DESC
```

###
