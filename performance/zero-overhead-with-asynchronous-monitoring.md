# Zero Overhead with Asynchronous Monitoring

Using Thundra, you can monitor and troubleshoot your serverless-centric system and get insights about the whole architecture. Thundra allows you to instrument your application with no code change. You can send  these monitoring data to have these insights in two ways:

* Synchronous monitoring&#x20;
* Asynchronous monitoring

To send data synchronously to Thundra, an HTTP call to Thundra's collector is made by your Lambda function before an invocation ends. This method is quite easy, but also risky. Should an HTTP call fail, latency could occur and affect your Lambda function's performance. In addition, making an HTTP call will add a duration overhead to the execution time of your Lambda function.&#x20;

By contrast, asynchronous monitoring requires zero overhead and trace, metric, and log data is structured in JSON format and then written to CloudWatch after data is gathered by Thundra's AWS integration in your account. This means that while observability data becomes visible in your Thundra console at a slower rate than through synchronous monitoring (depending on when it appears in CloudWatch),  you're also getting rid of communication overhead, which can go up to 100ms when data is too large.

Configuring asynchronous monitoring is quite simple. Just set the following environment variable to true in your Lambda function and make sure that you're using Thundra's AWS integration:

`thundra_agent_lambda_report_cloudwatch_enable`

If you aren't using AWS integration, you'll need to install the following Lambda function:

&#x20;`thundra-lambda-adapters-cw.`

There are three ways of installing `thundra_lambda-adapters_cw` to your AWS account:

* [Manual Setup](zero-overhead-with-asynchronous-monitoring.md#manual-setup-of-async)
* [Using Serverless Framework](zero-overhead-with-asynchronous-monitoring.md#serverless-framework)
* [Using Deployer Tool](zero-overhead-with-asynchronous-monitoring.md#deployer-tool)

### Manual Setup of Async

#### **1.** Setup “thundra-lambda-adapters-cw”

Thundra provides an AWS CloudFormation template to set up the `thundra-lambda-adapters-cw` Lambda function with the required configurations, permissions, and roles on the user’s end.

* Access CloudFormation on AWS console.
* Click `Create Stack`

![](<../.gitbook/assets/image (194).png>)

* Select `Specify an Amazon S3 template URL` and set to [https://s3-us-west-2.amazonaws.com/thundra-dist/thundra-lambda-adapters-cw-cloudformation-template.yaml](https://s3-us-west-2.amazonaws.com/thundra-dist/thundra-lambda-adapters-cw-cloudformation-template.yaml) and click `Next`

![](<../.gitbook/assets/image (291).png>)

* Enter the stack name, such as `thundra-lambda-adapters-cw-stack`, configure memory size and timeout parameters if needed, and then click “`Next`.”

![](<../.gitbook/assets/image (205).png>)

* No need to add extra options, so click `Next.`

![](<../.gitbook/assets/image (133).png>)

* Select `I acknowledge that AWS CloudFormation might create IAM resources with custom names.` under `Capabilities` section and click `Create`.

![](<../.gitbook/assets/image (262).png>)

* Stack creation will begin. Wait until the creation process ends.

![](<../.gitbook/assets/image (132).png>)

![](<../.gitbook/assets/image (250).png>)

#### 2. Subscribe to Lambda functions to be monitored

Go to the `thundra-lambda-adapters-cw` function in the AWS Lambda console. For each Lambda function that needs to be monitored:

* Add `CloudWatch Logs` trigger

![](<../.gitbook/assets/image (125).png>)



* Configure trigger
  * **Log Group**: Name of the log group of the Lambda function to be monitored. Log group is in the `/aws/lambda/<function-name>` format.
  * **Filter Name**: Enter the desired name of the filter
  * **Filter Pattern**: Pattern to filter AWS CloudWatch logs to accept only formatted monitor data. It must be `{$.type = Invocation || $.type = Trace || $.type = Span || $.type = Metric || $.type = Log || $.type = Composite}`\
    and click `Add`

![](<../.gitbook/assets/image (284).png>)

* Then click `Save` to apply the changes.

### Serverless Framework

You will need to install our custom serverless plugin. However, prior to installation, please remember that:

{% hint style="info" %}
External plugins are added on a per-service basis and are not applied globally. Make sure you are in your service's root directory before installing the corresponding plugin with the help of NPM.
{% endhint %}

To begin, go to your service’s directory and then:

* Install the plugin by `npm install --save serverless-plugin-thundra-lambda-adapters-cw`
* Import the plugin in your `serverless.yml`

```yaml
...

plugins: 
  - serverless-plugin-thundra-lambda-adapters-cw
  …

...
```

* Enable your lambda to publish on CloudWatch

```yaml
functions: 
    my-function: 
        ...
        environment:
            ...
            thundra_agent_lambda_report_cloudwatch_enable: true
            ...
        ...
... 
```

* Set the API key with the `thundra_apiKey` environment variable, as explained here:  [https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw/blob/master/README.md#usage](https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw/blob/master/README.md#usage)

```yaml
functions: 
    my-function: 
        ...
        environment:
            ...
            thundra_apiKey: <my-awesome-api-key> 
            ...
        ... 
    ...
```

* Alternatively, you can configure the thundra-lambda-adapters-cw Lambda function, as explained here:  [https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw/blob/master/README.md#configuration](https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw/blob/master/README.md#configuration)

```yaml
custom: 
    ... 
    thundraAdapterFunctionMemorySize: 1536
    thundraAdapterFunctionTimeout: 300 
    ...
```

* By default, all functions are monitored over AWS CloudWatch. If you want to exclude specific functions, you need to mark them by setting the thundraIgnored flag to true, as explained here:  [https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw/blob/master/README.md#usage](https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw/blob/master/README.md#usage)

```yaml
functions: 
    my-function: 
        ... 
        thundraIgnored: true 
        ... 
    ...
```

*   See the following page for more details:  [https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw](https://github.com/thundra-io/serverless-plugin-thundra-lambda-adapters-cw)

    **Note**: If you get an error like this

```yaml
...

Serverless: Operation failed!
 
  Serverless Error 
  ---------------------------------------
 
  An error occurred: ...LogGroup - /aws/lambda/... already exists.

...
```

You should skip the **log group creation** process for the mentioned function or for all functions. By default, this plugin creates log groups of monitored functions to subscribe them to Thundra. But if the log group was already created without this plugin (by invocation or manually), log group creation should be skipped. If you don’t skip the log group creation, you will get the “log group already exists” error shown above.

* To skip log group creation for a specific function, you can set the `thundraSkipLogGroupCreation` flag individually.

```yaml
functions: 
    my-function: 
        ... 
        thundraSkipLogGroupCreation: true 
        ... 
    ...
```

* To skip log group creation for all monitored functions, you can set the `thundraSkipAllLogGroupCreations` property globally.

```yaml
custom: 
    ... 
    thundraSkipAllLogGroupCreations: true 
    ...
```

### Deployer Tool

Download the tool from `https://s3-us-west-2.amazonaws.com/thundra-dist/thundra-lambda-adapters-cw-deployer.jar`

This tool has the following usage format:

```yaml
java -jar thundra-lambda-adapters-cw-deployer.jar 
    -mode=(adapter|subscription|full|export) 
    -region=<region> 
    [-profile=<profile>] 
    [-memory=<memorySize>] 
    [-timeout=<timeout>]  
    [(-functionNames=func1,func2,...) | (-functionNamesFile=functionNamesFile.txt)]
```

* **mode**: Specifies the mode of the deployer tool. This property is mandatory and can be one of the following values:
  * **adapter**: Deploys and sets up the `thundra-lambda-adapters-cw` Lambda function.
  * **subscription**: Subscribes the `thundra-lambda-adapters-cw` Lambda function to the log groups of Lambda functions needing to be monitored.
  * **full**: Executes both the adapter and subscription modes sequentially.
  * **export**: Executes both the adapter and subscription modes sequentially.&#x20;
  * **without**: Performs actual deployment but just exports generated AWS CloudFormation template files (`thundra-lambda-adapters-cw-cloudformation-template.yaml` and `monitored-function-resource-def-cloudformation-template.yaml`) to the directory where you run the deployer tool.
* **region**: Specifies the AWS region to deploy. This property is mandatory.
* profile: Specifies the AWS profile defined in the `~/.aws/credentials`. This property is optional.
* **memory**: Specifies the memory size of the `thundra-lambda-adapters-cw` Lambda function. This property is optional, and the default value is 512 MB.
* **timeout**: Specifies the timeout value of the `thundra-lambda-adapters-cw` Lambda function. This property is optional, and the default value is 300 seconds (5 minutes).
* **functionNames**: Specifies the names of the functions to be subscribed for monitoring. Multiple function names can be separated by comma (,) without a space. This property is mandatory if the mode is either subscription or full, and if the functionNamesFile property is not specified.
* **functionNamesFile**: Specifies the path of the file containing the names of the functions to be subscribed for monitoring. Multiple function names can be separated by a comma (,). This property is mandatory if the mode is either subscription or full, and if the functionNames property is not specified.

For example:

* Deploy `thundra-lambda-adapters-cw` into region `us-west-2`:\
  `java -jar thundra-lambda-adapters-cw-deployer.jar -mode=adapter -region=us-west-2`
* Subscribe `thundra-lambda-adapters-cw` to `my-func-1` and `my-func-2` at region `us-west-2`:\
  `java -jar thundra-lambda-adapters-cw-deployer.jar -mode=subscription -region=us-west-2 functionNames=my-func-1,my-func-2`
* Do the examples above at one step:\
  `java -jar thundra-lambda-adapters-cw-deployer.jar -mode=full -region=us-west-2 functionNames=my-func-1,my-func-2`

#### 1.Setup “thundra-lambda-adapters-cw”

* Run the deployer tool with the `adapter` mode by `-mode=adapter` argument, as explained above.
* Wait until the application terminates.

#### 2.Subscribe to Lambda functions to be monitored

* Run the deployer tool with the `subscription` mode by `-mode=subscription` argument, as explained above.
* Wait until the application terminates.

#### 3.Full setup

*   Run the deployer tool with the `full` mode by `-mode=full` argument, as explained above.

    Wait until the application terminates.

