# Query Helper for APIs



When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for APIs has four different sections:

* API Filters - Filters APIs according to specific properties for APIs.
* Statistical Filters - Filters APIs that display performance metrics according to the criteria you set.
* Sort Parameters - Sorts the list of APIs by the sorting criteria you set.
* Cheat Sheet - Provides examples of queries for your APIs list.

### API Filters

API filters allow you to filter your APIs via specific property parameters. For example, you could use the specific parameters for APIs to obtain all the APIs that have a specific runtime or are from a specific project. The parameters available are explained in greater detail below.

**Region** - Refers to the AWS region where your API resides, which includes all the AWS regions commercially available for APIs. You can find a full list of regions available for APIs. You can filter APIs in a specific region or group of regions.

**AWS Account Number** - Filters APIs according to their AWS account.

**Name** - Filters APIs that include one of the selected API names.

### Statistical Filters

Statistical filters allow you to filter through your APIs using exact numerical values related to the various runtime performances of APIs. These runtime performances include the following:

* Request Count
* Erroneous Request Count
* Average/P90/P99 Duration

![](<../../.gitbook/assets/image (13).png>)



Using statistical parameters in the query involves four fields that you can set:

* Operator - First, select the operator type from the Operator drop-down menu. Options include: AVG, COUNT, PERC90/99.&#x20;
* Metric - The Metric field will display the appropriate metric parameters to select from.
* Relation - After selecting the metric of your choice, you can choose the relation for the criteria to work on from the Relation field. `<,>,>=,<=,=`
* Value - Finally, fill in the value to filter by in the Value field. Click on the plus button to add more criteria.

### Sort Parameters

This option allows you to order your list of APIs according to the parameters you set. Similar to the statistical filters, setting the operator you would like to sort by changes the metrics that you can choose from, which are also similar to those available with the statistical filters option. The final field allows you to choose whether you would like to sort by ascending or descending order. The following metrics are available:

* Request Count
* Erroneous Request Count
* Average/P90/P99 Duratio

Click on the plus button to add more criteria.

### Cheat Sheet

If you have trouble writing a query for your APIs, you can take a look at the queries that we automatically prepare for you. Click on the "Go to Cheat Sheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your APIs using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (40).png>)
