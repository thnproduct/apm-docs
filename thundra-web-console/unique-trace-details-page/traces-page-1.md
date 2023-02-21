# Traces Tab

![](<../../.gitbook/assets/image (90).png>)

On the Traces page, you can list traces of your unique flow. You can navigate to this page by clicking a unique trace from the Unique Traces page and selecting the Trace List tab.

The Traces page includes four different sections:

* [Traces List](../traces-page.md#traces-list)
* [Queries](../traces-page.md#queries)
* [Query Bar](../traces-page.md#query-bar)
* [Trace Details](../traces-page.md#trace-details)

### Traces List

IOn the Traces page, traces are listed in terms of:

* Trace ID - ID of the trace
* Start Time - The start time of the trace
* Duration - End-to-end duration of the entire trace
* Errors - Types of the thrown-any error from any Lambda function in the entire trace

### Query Bar

The Query Bar on the Traces page allows you to write custom queries to filter your traces. You can save your queries to easily apply custom filters to traces. You can use the [Query Helper](../query-helper/query-helper-for-unique-traces.md) to write your own queries by selecting the parameters you would like to use to filter your data.

#### Query Save Details

If your role in the organization is designated as `Admin` or `Account Owner`, you can save queries as public so that everyone in the organization can see the saved query. You can still save some queries as private so that only you can see them. If your role is designated as `User` or `Developer`, you can only save the query for yourself and it won't be visible to the whole organization.

#### Saving Queries as Alert Policies

If your role in the organization is `Admin` or `Account Owner` or `Developer`, you can save queries as alert policies. The dropdown menu is not visible to the `User` role.

### Queries

In the Queries section, you can view a list of predefined queries that are created for you by Thundra. These predefined queries help you to list and filter your traces for different purposes.

You can also display your saved queries for your traces. Use the save button next to the Query Bar to save queries. You can set any query as your default, which will be run when you open the Traces page. You also have the ability to delete your saved queries.

### Trace Details

![Trace Map](<../../.gitbook/assets/image (293).png>)

When you click on a trace in the Traces List, you can display trace details as a Trace Map. It provides a visual look at a transaction with a flow-chart representation, helping you to easily understand a specific trace.&#x20;



![Filters](<../../.gitbook/assets/image (147).png>)

You can customize your architecture view using filters. When you hover your mouse over the Filters button on the Architecture page, the following options will be displayed:

* Show Labels, which is selected by default and shows labels on vertices. Specifically, for Lambda vertices, you can see the function name and the average duration of invocations.

![Show labels](<../../.gitbook/assets/image (200).png>)

* _Show Metrics,_ which shows more information about the interaction between a Lambda function and other resources. Without clicking on the edges, you can see the count and duration between your function and resources at a glance.

![Show metrics](<../../.gitbook/assets/image (185).png>)

* _Old AWS Icons_ shows your serverless architecture with old icons. Since AWS announced new icons very recently, you will need to uncheck this setting in order to see the new icons in your architecture.

![New icons](<../../.gitbook/assets/image (306).png>)

* _Export PNG_ exports your architecture image in png format.

![Lambda Trace Details](<../../.gitbook/assets/image (222).png>)

When you click on a **Lambda** function in the Trace Map, you can take a closer look inside of a Lambda invocation. This allows you to display details of the invocation, as shown below. These details include:

* Summary - Overall information about the span, including request and response data.
* Tags - Specific information passed with the span, even including custom tags if configured with custom spans.
* Logs - All the logs that occur with the specific part of the Lambda function represented in the span.

![SQS Messages](<../../.gitbook/assets/image (231).png>)

If you click on non-Lambda, you will see messages exchanged on this service. For example, if the SQS node is clicked, INBOUND and OUTBOUND messages will be displayed.

Clicking on an edge which will show the message, which flows on the edge on the right side of the screen.

### Required Thundra Library Versions

In order to have the Trace Map with your functions, you need to update your agent versions as follows:

* For Java, the agent library version is `2.2.0` or higher. The layer version needs to be `10` or higher.
* For Node.js, the agent library version is `2.3.0` or higher. The layer version needs to be `11` or higher.
* For Python, the agent library version is `2.3.0` or higher. The layer version needs to be `7` or higher.
* For Go, the agent library version is `2.1.0` or higher.

### Agent Configurations for Traces

By default, Amazon SQS, Amazon SNS, Amazon DynamoDB, AWS Lambda, and HTTP /Amazon API Gateway messages are shown when you click on their nodes.

**Amazon SQS:**

* `thundra_agent_lambda_trace_integrations_aws_sqs_message_mask`: Masks an SQS message sent on the client-side, which calls AWS SDK if it is `true.`

**Amazon SNS:**

* `thundra_agent_lambda_trace_integrations_aws_sns_message_mask`: Masks an SNS message sent on the client-side, which calls AWS SDK if it is `true.`

**Amazon DynamoDB:**

* `thundra_agent_lambda_trace_integrations_aws_dynamodb_statement_mask`: Masks DynamoDB statements (query, scan, get, put, modify, delete, etc ... requests) sent on the client-side, which calls AWS SDK if it is `true.`

**AWS Lambda:**

* `thundra_agent_lambda_trace_integrations_aws_lambda_payload_mask`: Masks Lambda invocation payload sent on the client-side, which calls AWS SDK if it is `true.`

**HTTP / API Gateway**:

*   `thundra_agent_lambda_trace_integrations_http_body_mask`: Masks an HTTP request body sent on the caller side if it is `true`.

    This behavior is disabled for Kinesis, Firehose, and CloudWatch logs by default. You can change this by adjusting the following variables from the environment variables:

**Amazon Kinesis:**

* `thundra_agent_lambda_trace_integrations_aws_kinesis_record_unmask`: Traces sent Kinesis record at client side which calls AWS SDK if it is `true`.
* `thundra_agent_lambda_trace_kinesis_request_enable`: Traces incoming Kinesis record at triggered Lambda side if it is `true`.

**Amazon Firehose:**

* `thundra_agent_lambda_trace_integrations_aws_firehose_record_unmask`: Traces sent Firehose record at client side which calls AWS SDK if it is `true`.
* `thundra_agent_lambda_trace_kinesis_request_enable`: Traces incoming Firehose record at triggered Lambda side if it is `true`.

**Amazon CloudWatch Log:**

* `thundra_agent_lambda_trace_cloudwatchlog_request_enable`: Traces incoming CloudWatch log message at triggered Lambda side if it is `true`.
