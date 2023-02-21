# Operations Page

![Operations Page Breakdown](<../.gitbook/assets/image (277).png>)

Thundra has capabilities to search through functions, invocations, or traces using flexible queries. However, to search for an invocation or trace, you must first search for a function – then you will be able to display the details of an invocation or trace. The Thundra Operations page allows you to search through operations of these functions.&#x20;

You can navigate to the Operations page by clicking the following icon on the left-hand side bar.

![Operations Page Icon](<../.gitbook/assets/image (245).png>)

The Operations page consists of the following sections:

* Queries
* Query Bar
* Operations List

### Queries

In the Queries section, you can view a list of predefined queries that are created for you by Thundra. These predefined queries help you to list and filter your operations for different purposes.

You can also display your saved queries for your operations. Use the save button next to the Query Bar to save queries. You can set any query as your default, which will be run when you open the Operations page. You also have the ability to delete your saved queries.

### Query Bar

The Query Bar on the Operations page allows you to write custom queries to filter your operations. You can save your queries to easily apply custom filters to operations. Another option is to save your operation queries as `Alert Policies`. You may want to check the Alert Policies page for more detailed information. You can use the [Query Helper](query-helper/query-helper-for-operations.md) to write your own queries by selecting the parameters you would like to use to filter your data.

#### Query Save Details

If your role in the organization is designated as `Admin` or `Account Owner`, you can save queries as public so that everyone in the organization can see the saved query. You can still save some queries as private so that only you can see them. If your role is designated as `User` or `Developer`, you can only save the query for yourself and it won't be visible to the whole organization.

#### Saving Queries as Alert Policies

If your role in the organization is designated as `Admin`, `Account Owner`, or `Developer`, you can save queries as alert policies. The dropdown menu is not visible to the User role.

### Operations List

On the Operations page, operations are listed in terms of:

* Resource Name - Name of the resource (Dynamo DB, table name, etc.) that the operation is based on. When an error occurs in an operation, there will be a badge next to the resource name.
* Resource Type - Type of resource that the operation is based on (Dynamo DB, SNS, SQS etc.).
* Application Name - Lambda function name of the performed operation.
* Start Time - Start time of the operation.
* Duration - Duration of the operation.

When an operation is clicked on in the Operations List, the [Invocation Details](invocation-detail-page/) page for that operation will open, the [Trace Chart](invocation-detail-page/trace-chart-tab.md) tab on that page will open, and the operation will be selected by default on the Trace Chart.
