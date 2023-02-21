# Logs Tab

![](<../../.gitbook/assets/image (58).png>)

Logs and console logs of your Lambda function for a specific invocation are listed in the Logs tab.

These log and console messages are displayed chronologically, and you also have the option to search through these messages. The tab view itself is quite simple with only two components: the Log List and the Search Bar. By clicking on the CloudWatch Logs button, you will be redirected to your AWS account to display CloudWatch Logs.

To navigate to the logs, you first have to select a specific invocation to analyze, which will get you to the Invocation Details page. On this page, you can access all your log data by selecting the Logs tab.

![Logs Tab Selected](<../../.gitbook/assets/image (271).png>)

### Log List

The Log List is the view where all console messages and logs of your Lambda function are listed. There are three main parts to a message listing:

* Time the message was logged
* Level of the logged message
* The message itself

The trace levels of the log are color-coded according to the priority of the level. Hence, lower levels are displayed in bland, dark colors while higher levels are displayed in brighter colors. When reading logs, it is always a good practice to know the priority numbers of the different levels of the log, according to the runtime of the Lambda function.

### Search Bar

The Search Bar is located at the top right of the Logs tab, and allows you to search your list of logs by content.
