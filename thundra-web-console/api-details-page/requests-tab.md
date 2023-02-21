# API Requests Tab

The Requests tab lists all of the requests of a specific API. You can filter through these requests and get detailed information via their metrics.

![](<../../.gitbook/assets/image (74).png>)

Requests are listed in terms of:

* Endpoint Name - Name of the endpoint that receives the request.
* Method - Type of the endpoint that receives the request. For the API-Gateway APIs, this could be GET/POST. For the AppSync APIs, this could be Query or Mutation.
* Start Time - Start time of the request.
* Duration - Execution time of the request.
* Status - Status code of the request.
* Error - Whether or not the request is erroneous.

### Query List

The Query List component, which is located on the left-hand side of the Requests tab, lists all queries that you can use to filter requests. It is split into two sections: Predefined Queries and Saved Searches.

The Predefined section lists queries that are created by Thundra to allow you to quickly and conveniently filter your requests. The following predefined queries are included:

* Long-Running requests - Requests that have long durations.
* Erroneous Requests - Requests with errors.
* Latest Requests - Requests are ordered by start time.

The Saved Searches section of the Query List shows all the queries that you would like to save after entering them into the Query Bar or the Query Helper.

To use a query in the Queries List, simply select one, and the request listed will be filtered and ordered depending on that query. By clicking or hovering your mouse over a query, you will see two buttons appear to the right side of the query, allowing you to either delete it or set the query as the default. This will filter your functions according to the default query whenever you visit your Thundra console.

### Query Bar

The Query Bar, which is located above the listed requests, allows you to write custom queries to filter through your requests. Three buttons are present at the end of the Query Bar, and they allow you to run, share, and save the written query respectively.

The Run button will execute whatever query is entered in the bar. The Save button will open up a drop-down menu, where you can save the query using the Save Search Dialog. Click on the  "Share" button to share your queries with your colleagues. That will copy the URL of the query and when you team members use this URL, shared query will automatically run.

{% hint style="info" %}
#### Query Save Details

If your role is designated as Admin or Account Owner, you can save a query as public so that everyone in the organization can view it. You can also save a query as private so that only you can see it. If your role is designated as User or Developer, you can only save the query for yourself and it won't be visible to the whole organization.
{% endhint %}

### Trace Map

When you click on a request, you will be redirected to a new page, and the trace map of the request will be displayed.When you click on a request, you will be redirected to a new page where the request’s trace map will be displayed.

Click on the nodes to display details of the request. When you select a node, it will be shown on the trace chart in the panel on the right-hand side. Details of a span are displayed under this trace chart. You can also use a resource’s tags to have insight into a request.
