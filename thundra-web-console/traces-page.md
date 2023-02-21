# Unique Traces Page

![](<../.gitbook/assets/image (42).png>)

Serverless architectures enable developers to build resilient distributed applications in which messages are exchanged between different functions, and create a business transaction to achieve a certain business goal. Out-of-the-box solutions provide visibility using functions, but it is hard to track a business transaction happening in a serverless architecture.

Thundra provides developers the ability to track business flows via the Unique Traces page. Traces are basically a chain of several applications, such as AWS Lambda or a container application. A distributed trace can occur multiple times in a system. We can describe these traces as a business flow that achieves a job, but listing all of the traces does not help you to understand and keep track of your KPIs. That’s why Thundra automatically detects and lists unique flows that your application performs under Unique Traces!

You can navigate to the Traces page by clicking on the Unique Traces icon button on the left side of the bar.

![](<../.gitbook/assets/image (233).png>)

### Unique Traces List

On the Unique Traces page, unique flows are listed in terms of:

* Alias - Name of the unique trace, which is created by combining the resource name of the trigger of the transaction with the main Lambda function's name. You can click on the pencil icon to give a name to your unique trace. Press enter to save the name.

![Edit unique trace name](<../.gitbook/assets/image (178).png>)

* Entry point - The Lambda function that is first executed on the transaction.
* Trace count - Number of times this unique trace is visited.
* Error count - Number of erroneous visits of the unique trace.
* Average duration - Average end-to-end duration of a unique trace.
* P50 duration - 95th percentile duration
* P99 duration - 99th percentile duration
*   Resources - Resources interacted in a unique trace. Hover your mouse over a resource to display its name.



![](<../.gitbook/assets/image (47).png>)

Filter your unique traces using Query Helper. You can search through unique traces by name using the Search Bar at the top of the list.

Click one of the Unique Traces to get more information about it on the [Unique Trace Details Page](unique-trace-details-page/).

###
