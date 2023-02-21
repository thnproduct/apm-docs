# Applications Page

The Applications page gives you an overview of your applications that are connected to Thundra. This is one of the main pages of Thundra, allowing you to view the performance of an application with just a glance.

The Applications page displays all your instrumented applications according to the query you set using the Query Bar and Query Helper, located at the top of the page. You can also change the time constraint on the functions you are displaying using the “Time” option next to the Query Bar.

![](<../.gitbook/assets/image (46).png>)

As seen in the illustration above, the Applications page can be broken down into various components that help you filter through your applications and view all data for an individual application. These components are:

* Query List
* Query Bar
* Search
* Applications List

### Query List

The Query List component, which is on the left side of the Applications page, lists all the queries that you can use to filter the displayed applications. It is split into two sections: Predefined Queries and Saved Searches.

The Predefined section lists queries that are created by Thundra to allow quick and convenient filtering of your applications. The following predefined queries are included:

* Most Erroneous - Orders applications by error rate in descending order
* Applications with the Highest Throughput - Orders applications by throughput in descending order
* Most Called - Orders applications by request count in descending order

The Saved Searches section of the Query List shows all the queries that you would like to save after entering them into the Query Bar or the Query Helper.

To use a query in the Queries List, simply select one and the applications listed will be filtered and ordered depending on which query you selected. Moreover, by clicking or hovering your mouse over a query, you will see two buttons appear to the right of the query, allowing you to either delete it or set it as the default query. The latter option will filter your functions by default whenever you visit your Thundra console.

### Query Bar

The Query Bar, which is located above the listed applications, allows you to write custom queries to filter through your applications. There are three buttons at the end of the Query Bar, which allow you to run, share, or save the written query.

The Run button will execute whatever query is entered in the bar. The Save button will allow you to save the query, and the Save Search Dialog will allow you to name the query. You can share your query with your colleagues using the “Share” button, which will copy the URL of the query.

{% hint style="info" %}
#### Query Save Details

You can save your queries as public if your role is designated as Admin or Account Owner, meaning everyone in the organization can see the saved query. You can also save your queries as private so that only you can see the saved query. If your role is designated as User or Developer, you can only save the query for yourself and it will not be visible to the whole organization.
{% endhint %}

### Search

The Search Bar, located at the top of your applications list, allows you to see a list of your applications based on their names.&#x20;

### Applications List

You can display your applications list after you filter or order them using queries. Applications are listed in terms of:

* Application name
* Application type - Indicates web framework used for your application, such as Spring, Express.js, etc.
* Throughput - Shows throughput of your application request count per second
* P50 Latency
* P90 Latency
* P99 Latency
* Error Rate - Displays the percentage of erroneous requests
