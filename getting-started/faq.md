# FAQ

#### What does zero overhead mean? How can I achieve that with Thundra?

If Thundra gathers data synchronously through Lambda calls, it will create some delay to your Lambda function execution. Enabling asynchronous monitoring will remove this additional execution time and you can use Thundra without any overhead. We call this solution “zero overhead.” You can check out [this part](../performance/zero-overhead-with-asynchronous-monitoring.md) of our documentation to learn how to set up asynchronous monitoring.\


#### Will my function take longer than before?&#x20;

No. Once you set up asynchronous monitoring, our collector module will start to collect the monitoring data asynchronously. According to our benchmarks, we only add 2 ms to the invocation time on average. This capability is unique to Thundra, and we are proud of it.

If you need to send your data synchronously, it will add minimal overhead to your application, which is 5 ms on average. We are continuously working to reduce overhead, and you can see our latest benchmarks [here](performance-benchmarks.md).&#x20;

#### Will I experience loss of monitoring data?&#x20;

No. Systems using synchronous monitoring should have a retry mechanism to make sure there is no data loss, as data sending may fail in some attempts. Retries result in latency. If there is no retry mechanism, data loss could occur. Thundra solves these issues thanks to its asynchronous monitoring capability.&#x20;

#### Can I hide or encrypt the monitoring data before sending Thundra?&#x20;

Yes. By default, requests and responses of invocations are not sent to Thundra. You can enable or disable this feature using environment variables. You can also manage masking objects, arguments, return values, and local variables by serializing them using programmatic API. You can read more about this topic [here](broken-reference).

#### Can I use Thundra for my functions within VPC?

Yes. Thundra doesn’t run along with your Lambda function; instead, its tracing capabilities are injected into your function and log collection happens asynchronously through Amazon CloudWatch. That’s why VPC rules don’t apply to Thundra with asynchronous monitoring. For more information about all other unique advantages of asynchronous monitoring, check out our blog post: [https://medium.com/thundra/4-reasons-why-you-should-publish-monitoring-data-async-in-aws-lambda-fd1e56473941](https://medium.com/thundra/4-reasons-why-you-should-publish-monitoring-data-async-in-aws-lambda-fd1e56473941)

#### What is the point of API Keys?&#x20;

In Thundra, API keys are required to send and monitor data. API keys are used for authentication, identifying requests, and applying rate limiting to the requests (as needed). In order to serve users better, we had to put a limit on the number of requests per account. For now, the limit is set as 1,000 requests per minute.

#### Can I distribute my request limit within my organization?&#x20;

Yes. You can create multiple API keys to distribute your total request count to different teams within your organization. Note that the total limit for multiple API keys is still 1,000 requests per minute.

#### What if my requests exceed the limit?&#x20;

In this case, the exceeding requests are simply dropped. If you are using synchronous monitoring, you will face HTTP errors. If you are using asynchronous monitoring, you will see the error logs in CloudWatch logs.

#### I hit the limits of the plan, what will happen now?

When you hit the limits of your plan, we block your trace data. You can still continue to use Thundra; however, you can only monitor your invocation data.

When you double your limits without using tracing capabilities, you exceed your data or invocation allotment. That's why we restrict your Thundra usages. Until the next period of your plan, Thundra won't accept data and invocation. You can upgrade your plan on the [billing page](../thundra-web-console/profile/billing-page/) to continue using Thundra.

You can track your usage on the [Report tab](../thundra-web-console/profile/billing-page/report-tab.md) of billing page.

#### I have executed my AWS Lambda function but couldn’t see its metrics in the dashboard instantly‒ Why?

It takes 1-2 minutes to see your function’s monitoring data in the application.\


#### Can I use Thundra for AWS Lambda functions in different programming languages?&#x20;

Thundra currently supports Lambda functions in Java, Node.js, Python, Go, and .NET. Using the same API key, you can monitor functions in all of the supported languages.\


#### Thundra cannot subscribe to log group of the function. Why?

CloudWatch only allows one subscription per log group. If the log group already has a subscription filter (such as Elasticsearch subscription filter or AWS Lambda subscription filter), you have to remove the existing filter, as shown below.\


![Remove subscription filter on new UI](<../.gitbook/assets/image (154).png>)

![Remove subscription filter on old UI](<../.gitbook/assets/image (179).png>)

#### How can I delete my Thundra account?

You can delete your Thundra account and data stored in Thundra on the [Profile tab](../thundra-web-console/profile/settings-page/profile-tab.md) of the Settings page. If you have AWS accounts connected to the Thundra, you need to disconnect AWS accounts first then you can delete your account. You can visit the [AWS Tab](../thundra-web-console/profile/settings-page/aws-tab/) and disconnect or you can simply delete the Thundra CloudFormation stack from your AWS account. We will remove all data and your account information in 24 hours permanently.

#### I couldn’t find an answer to my question. How can I get help?

Just join our [Slack channel](https://join.slack.com/t/thundra-community/shared\_invite/enQtMzA5OTMxNzg1OTUyLWFmYTZjNmI4MTM0YzViM2E4MTI1YmUzNzE2MTQ2ZWFiZGJhYmExM2MzYTU1ZGJiODQ3MmJhMDk2MmQ1NThjNDY), we'll be glad to assist you.
