# APIs Page

![](<../.gitbook/assets/image (92).png>)

The APIs page provides you with a list of AppSync APIs on your AWS account, giving you deep insight into your APIs and the ability to monitor them. You can view the details of an API on this page, including endpoints, requests, and metrics.&#x20;

To navigate to the APIs page, click on the API icon on the left-hand sidebar.

![APIs Page Icon](<../.gitbook/assets/image (69).png>)

Monitoring your APIs is very easy and straightforward. Just connect your AWS account to Thundra using the CloudFormation template, and you are ready to begin! You can find detailed information about how to connect your AWS account to Thundra [here](../getting-started/quick-start-guide/connect-thundra.md).&#x20;

If you want to monitor only AppSync APIs, you don't need to select any function during AWS integration. After AWS integration is completed, go to the Settings page of your AppSync API on the AWS console, and enable logs so that Thundra can monitor your AppSync APIs. A few minutes later, you will be able to see your AppSync APIs on this page.

APIs are listed with the following basic information:

* API Name - The name of the API that you create during configuration.
* Request Count - Count of the requests received within the selected time period.
* Error Count - Number of erroneous requests within the selected time period.
* Average Duration - Average duration of the requests.
* P90 Duration - P90 duration of the requests.
* P99 Duration - P99 duration of the requests.

#### Query List

The Query List component, which is on the left-hand side of the APIs page, lists all queries that you can use to filter the displayed APIs. It is split into two sections: Predefined Queries and Saved Searches.

The Predefined section lists queries that are created by Thundra to allow quick and convenient filtering of your APIs. The following predefined queries are included:

* Long-Running APIs - Lists APIs that have average durations.
* Erroneous APIs- Lists APIs with erroneous requests.
* Most-Called APIs - Orders APIs by request count in descending order.

The Saved Searches section of the Query List shows all the queries that you save after entering them into the Query Bar or the Query Helper.

To use a query in the Query List, simply select one and the APIs listed will be filtered and ordered depending on that query. By clicking or hovering your mouse over a query, you will see two buttons appear to the right of the query, allowing you to either delete it or set the query as the default. This will filter your functions according to the default query whenever you visit your Thundra console.

#### Query Bar

The Query Bar, which is located above the listed APIs, allows you to write custom queries to filter through your APIs. Three buttons are present at the end of the Query Bar, and they allow you to run, share, and save the written query respectively.

The Run button will execute whatever query is entered in the bar. When you click on the Save button, the "Save Search Dialog" will allow you to name the query you wish to save. Click on the  "Share" button to share your queries with your colleagues. That will copy the URL of the query and when you team members use this URL, shared query will automatically run.

{% hint style="info" %}
#### Query Save Details

If your role is designated as Admin or Account Owner, you can save a query as public so that everyone in the organization can view it. You can also save a query as private so that only you can see it. If your role is designated as User or Developer, you can only save the query for yourself and it won't be visible to the whole organization.
{% endhint %}

#### Search Bar

The Search Bar allows you to see a list of your APIs based on their names, and is located at the top of your APIs list.

Click one of your APIs to see more information about it on the API Details page.
