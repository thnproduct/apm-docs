# Query Helper for Unique Traces

![](<../../.gitbook/assets/image (3).png>)

When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for unique traces has four different sections:

* [Unique Trace filters](query-helper-for-unique-traces.md#unique-trace-filters) - Filters unique traces according to specific properties for unique traces.
* [Statistical filters](query-helper-for-unique-traces.md#statistical-filters) - Filters unique traces which display performance metrics according to the criteria you set.
* [Sort parameters](query-helper-for-unique-traces.md#sorting-parameters) - Sorts the list of unique traces by the sorting criteria you set.
* [Cheat Sheet](query-helper-for-unique-traces.md#cheat-sheet) - Provides examples of queries for your unique traces list.

### Unique Trace Filters

Unique Trace Filters allow you to filter your unique traces via specific property parameters. For example, you could use the specific parameters for unique traces to obtain all the unique traces that have a specific runtime or from a specific project. The parameters available are explained in greater detail below.

**Region** - Refers to the AWS region where your Lambda function resides, which includes all the AWS regions commercially available for AWS Lambda. You can find a full list of regions available for AWS Lambda here. You can filter unique traces originated by a Lambda function in a specific region or group of regions.

**Runtime** - Refers to the AWS runtimes that are supported by Thundra. You can filter unique traces originated by a Lambda function with a specific runtime or group of runtimes.

**Project** - Considering that your Lambda functions are grouped by project, you can filter unique traces originated by a Lambda function in a specific project or group of projects.

**Origin Lambda** - Filters unique traces originated by specific Lambda functions using the start point of those unique traces. You can select more than one function for this option.

**Name** - Filters unique traces that include one of the selected Lambda functions.

### Statistical Filters

Statistical parameters allow you to filter through your unique traces using exact numerical values related to the various runtime performances of the unique traces. These runtime performances include the following:

* Trace Count
* Error Count
* Average Duration

![](<../../.gitbook/assets/image (111).png>)

Using statistical parameters in the query involves four fields that you can set:

* Operator - First, select the operator type from the Operator drop-down menu. Options include: AVG, SUM, COUNT, MIN, MAX, PERC50/90/95/99.&#x20;
* Metric - The Metric field will display the appropriate metric parameters to select from.
* Relation - After selecting the metric of your choice, you can choose the relation for the criteria to work on from the Relation field. `<,>,>=,<=,=`
* Value - Finally, fill in the value to filter by in the Value field. Click on the plus button to add more criteria.

### Sorting Parameters

This option allows you to order your list of traces according to the parameters you set. Similar to the Statistical Filters, setting the operator you would like to sort by changes the metrics that you can choose from, which are also similar to those available when you use Statistical Filters. The final field allows you to choose whether you would like to sort by ascending or descending order.&#x20;

* Trace Count
* Error Count
* Average Duration

Click plus button to add more criteria.

### Cheat Sheet

If you have trouble writing a query for your unique traces, you can take a look at the queries that we automatically prepare for you. Click on the "Go to Cheat Sheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your unique traces using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (21).png>)

Below are more example queries for you.

To list unique traces that contain an invocation of a specific application:

```
Name=user-team-api-java-lab ORDER BY COUNT(Error) DESC 
```

To list unique traces that have more errors than a set threshold (in this case 20):

```
COUNT(Error) >20 ORDER BY COUNT(Error) DESC
```

To list unique traces with at least one SQS interaction whose duration is longer than 100ms:

```
resource.AWS-SQS.duration>100 
ORDER BY COUNT(Trace) DESC
```

To combine two (or more) conditions together, you can use the “AND” keyword. For example, the following query will return unique traces that took longer than two seconds and that have at least one SQS interaction whose duration is longer than 100ms:

```
resource.AWS-SQS.duration=100 
AND AVG(Duration) >2000 
ORDER BY COUNT(Error) DESC
```
