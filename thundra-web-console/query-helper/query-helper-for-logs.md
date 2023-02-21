# Query Helper for Logs

![](<../../.gitbook/assets/image (70).png>)

When you start to write a query in the Query Bar, the Query Helper will be displayed to assist you.

The Query Helper for logs has three different sections:

* Log Filters - Filters logs according to specific properties for logs.
* Sort Parameters - Sorts the list of logs by the sorting criteria you set.
* Cheat Sheet - Provides examples of queries for your logs.

### Log Filters

Log Filters allow you to filter your logs via specific property parameters for logs. For example, you could use these specific parameters to obtain all the logs of a specific Lambda function. The parameters available are explained in greater detail below.

**Name**: Filters your logs by name of the function itself.

**Level**: Filters logs according to their log level. Some logs can be more critical or more mediocre compared to others. For example, `console.error()` and `console.log()` represent different levels of severity. You can also give your own log a level with Thundra loggers.&#x20;

**Message**: You can make a wild card search in the logs using wild cards or exact words. For example, you can type `*undr*` and this filter will list the logs which contain the words `Thundra` and `hundreds`.

### Sorting Parameters

These parameters allow you to order your list of logs according to the parameters set. There is only one option for logs:

* Time

You can choose to sort your logs list by time in ascending or descending order, or you can delete this parameter so it will not sort your logs.

### Cheat Sheet

If you have trouble while writing a query for your logs, you can take a look at the queries that we automatically prepare for you. Click on the "Go to CheatSheet" link while in Query Helper mode or manual mode to display examples of queries you can use. If you want to filter your logs using one of these queries, click on the play button next to any query.

![](<../../.gitbook/assets/image (32).png>)

Below are more example queries for you.

In order to list log items that have a log level of "INFO":

```
Level=INFO ORDER BY Time DESC
```

In order to list log items that have a string value similar to some string, you can make the following search:

```
Message=*err* ORDER BY Time DESC
```

In order to combine two (or more) conditions together, you can use the “AND” keyword. For example, the following query will return logs that have the "err" string in it and that have a log level of "INFO":

```
Message=*err* AND Level=INFO ORDER BY Time DESC
```
