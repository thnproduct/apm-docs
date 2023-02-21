# Logs page

![](<../.gitbook/assets/image (6).png>)

The oldest (but also the most effective) way of understanding a system is to check the logs to see if they contain something that is printed in case of a problem. Structured logging is particularly helpful if you want to troubleshoot as quickly as possible. However, checking for logs isn’t a straightforward process on CloudWatch, and you only have the capability to check the logs of one function at once. The Logs page solves this problem by giving you the flexibility to search the logs of all your serverless stacks.

You can navigate to the Logs page by clicking on the Logs icon button located on the left-hand sidebar.

![Logs Icon Button](<../.gitbook/assets/image (261).png>)

In the Logs page, logs are listed in terms of :

* Time - log time
* Function Name - Related function of log
* Actions - Allows you to go to the invocation that the log is included in.&#x20;
* Show more - If the message of the log is too long to display, clicking on this row will show you the entire message.

Using the time settings, you can change the time interval to display logs that were generated in that time period.

You can filter your logs using the Query Bar. These filters include:

* Log Level: Some logs can be more critical or more mediocre compared to others. For example: `console.error()` and `console.log()` represents different levels of severity. You can also make your own log level with Thundra loggers, and filter them according to their log level.
* Log Message: You can make a wild card search in the logs using wild cards or exact words. For example, you can type `*undr*` and this filter can bring you the logs which contain `Thundra` and `hundreds`.
* Log Context:  With this field, you can filter logs according to their source.
* Function Name: You can make a wild card search in the logs using wild cards or exact words.

&#x20;
