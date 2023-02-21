# Transactions Tab

The Transactions tab allows you to access every individual transaction executed by any endpoint of the application being analyzed. Transactions are listed in a table, which can be filtered with queries using the Query Bar or the Query Helper. You can also save your built queries in the Query List, located on the left-hand side of the Transactions tab.

To navigate to the Transactions tab, access the [Applications](../applications-page.md) page from the navigator bar on the left-hand side of the console. You can then select an application to analyze from the list displayed and go to the Application Details page, where you can select Transactions from the tab view.

![](<../../.gitbook/assets/image (4).png>)

You can also navigate to the Transactions tab from the "Endpoint" filter found on the Overview tab by selecting the "See all transactions" link in the transactions section.

The Transactions tab consists of the following components:

* Query List
* Query Bar
* Transactions Table

### Query List

The Query List, located on the left-hand side of the displayed transactions, lists all defined queries that you can use to filter through transactions. It can be split into two sections: Predefined queries and Saved Searches.

The Predefined section lists all queries that Thundra has created for you to provide quick access and filtering capabilities. These include:

* Long-Running Transactions - filters transactions that take longer than 1000 milliseconds.
* Erroneous Transactions - filters erroneous transactions and sorts them according to their start time in descending order.
* Latest Transactions - sorts transactions according to their start time, from newest to oldest.
* The Saved Searches section lists queries that you have entered and saved in the Query Bar.

To use a query in the Queries List, simply select one and the function listed will be filtered and ordered according to that query. Moreover, by clicking or hovering your mouse over a query, you will see two buttons appear to the right of the query. These buttons allow you to either delete the selected query or set it as the default query, meaning it will automatically filter your functions whenever you visit your Thundra console.

To save queries in the Saved Searches section, you need to provide the name and permissions of the system when prompted by the dialog.\


{% hint style="info" %}
#### Query Save Details

You can save your queries as public if your role is designated as `Admin` or `Account Owner`, so that everyone in the organization can see them. You can still save your queries as private so that only you can see them. If your role is designated as `User` or `Developer`, you can only save the query for yourself, which means your query won't be visible to the whole organization.
{% endhint %}

### Query Bar

The Query Bar section, which is located at the top of transactions, allows you to write custom queries to filter your transactions. There are three buttons at the end of the Query Bar which allow you to run, share, and save the written query respectively.

The “Run” button will execute whatever query is in the bar. The “Save” button will open up a dialog that will save your query in the Saved Searches section. You can share your query with your colleagues by clicking the “Share” button, which will copy the query’s URL.

### Transactions Table

When you’re examining a specific application, all its transactions are listed in the Transactions Table. These transactions are listed based on the query entered in the Query Bar or a query selected from the Query List. Transactions are listed in terms of:

* Endpoint - Name of the endpoint that starts the transaction.
* Method - Method of the endpoint can be GET, POST, etc.
* Start Time - Start time of the transaction.
* Latency - Execution time of the transaction.
* Status - Status code of the transaction.
* Error - If a transaction is erroneous, an error message will be displayed.

####
