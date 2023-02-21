# Dealing with Cold Starts

Cold starts have been a large problem for users since serverless began, which is why Thundra developed the warm-up plugin. This feature has helped thousands of serverless practitioners to eliminate the effects of cold starts.

### Installing Thundra's warm-up plugin

There are two ways to install Thundra's warm-up plugin: manually from our CloudFormation stack, or by using the Serverless Framework to add it into your functions.

#### Manual setup with CloudFormation

Thundra provides an AWS CloudFormation template to set up the thundra-lambda-warmup Lambda function with required configurations, permissions, and roles on the user’s end.

* Access CloudFormation on the AWS console.
* Click “Create Stack.”
* Select “Specify an Amazon S3 template URL” and set to: [https://thundra-public.s3-us-west-2.amazonaws.com/thundra-lambda-warmup/thundra-lambda-warmup-cloudformation-template.yaml](https://thundra-public.s3-us-west-2.amazonaws.com/thundra-lambda-warmup/thundra-lambda-warmup-cloudformation-template.yaml) and click `Next`
* Select `I acknowledge that AWS CloudFormation might create IAM resources with custom names` under the Capabilities section, and click “Create.”
*   When the installation of the CloudFormation stack is complete, you'll find a Java function among your Lambda functions with the name thundra\_lambda\_warmup. This function makes dummy requests to your other functions in order to keep their container warm and prevent cold starts.



#### Setup with Serverless Framework

First, you need to have the thundra-lambda-warmup artifact. You can do this with either of the following processes:

* Clone the repository from [https://github.com/thundra-io/thundra-lambda-warmup.git](https://github.com/thundra-io/thundra-lambda-warmup.git) and build with maven by `mvn clean install.`
* Download the thundra-lambda-warmup artifact from  [https://s3-us-west-2.amazonaws.com/thundra-dist-us-west-2/thundra-lambda-warmup.jar](https://s3-us-west-2.amazonaws.com/thundra-dist-us-west-2/thundra-lambda-warmup.jar)

After obtaining the artifact:

* If you have already cloned the repository, go to the root directory where the project has been cloned.
* If you downloaded the artifact instead:
  * Get the `serverless.yml` from [https://github.com/thundra-io/thundra-lambda-warmup/blob/master/serverless.yml](https://github.com/thundra-io/thundra-lambda-warmup/blob/master/serverless.yml)
  * Update the artifact configuration under the `package` section with the downloaded `thundra-lambda-warmup.jar` path on your local environment.

Next, specify the following mandatory configurations under the custom section in the `serverless.yml`:

* Configure the region where the `thundra-lambda-warmup artifact` will be deployed.
* Configure deploymentBucket with the S3 bucket where the `thundra-lambda-warmup` artifact will be deployed.
* Configure other configurations if desired (this is optional).

Finally, deploy the `thundra-lambda-warmup` artifact as a Lambda function by `sls deploy`.

### Configuring Thundra's warm-up plugin

If your AWS Lambda functions are wrapped by Thundra’s agent, no additional steps are needed; you just need to specify which Lambda functions should be monitored. There are two ways to do this:

* **Active way**: Lambda functions that need to be warmed up are specified by the `thundra_lambda_warmup_function` environment variable and separated by a comma. This environment variable is then put into the `thundra-lambda-warmup` Lambda function. For example, the environment variable key (name) is `thundra_lambda_warmup_function` and value is `my-func-1,my-func-2,my-func-3`.
* **Passive way**: Lambda functions that need to be warmed up declare themselves with the `thundra_lambda_warmup_warmupAware` environment variable (the value is `true`). Note that this environment variable is put into the Lambda function that should be warmed up (NOT into the `thundra-lambda-warmup` Lambda function).

By default, Lambda functions are warmed up with a concurrency factor `eight`, which is the number of warmed up container counts needing to be kept up. This factor can be configured with the `thundra_lambda_warmup_invocationCount` environment variable. There are a few points you should be aware of:

* This approach is the best way to keep multiple containers up. So this means that even though the concurrency factor is `eight`, there is no guarantee that there are exactly eight containers up; there may be more or less. But in practice, and based on previous tests we have run, generally eight containers will be up.
* In addition, even if there are active containers that are not busy with an invocation, AWS Lambda might dispatch the request to a new container. Because of this, cold starts may still occur.

To sum up, the `thundra-lambda-warmup` Lambda function can dramatically reduce your cold starts. However, keep in mind that it is not a complete fix, and there is no guarantee that there won't be any cold starts.

**NOTE**: Currently all agents (Java, Node.js, Python, Go, and .NET agents) have warm-up support.

See [here](https://github.com/thundra-io/thundra-lambda-warmup) for more details.

#### .Net Agent Specific Configurations

When enabling the warm-up functionality for your .Net Lambda functions using Thundra, make sure to implement the “IsEmpty” method when the “ISelfAware” class is extended to your Request object class.

For example, if your handler class extends the Thundra's Lambda Request Handler as shown below: `public class HandlerClass : LambdaRequestHandler<GetAlbumRequest, Album>` then you must extend “`ISelfAware`” in the GetAlbumRequest class.

```aspnet
using Thundra.Agent.Lambda.Core;

namespace dotnet
{
    public class GetAlbumRequest : ISelfAware
    {
        public int id { get; set; }

        public GetAlbumRequest()
        {
        }

        public bool IsEmpty()
        {
            return id.Equals(0);
        }
    }
}
```

{% hint style="info" %}
#### Non-Object Request

If you are passing something other than an object as a request (a string, for example), then you do not have to implement the `IsEmpty` method. The same goes for dealing with Stream handlers.
{% endhint %}

### Fast Start-up for Java Lambda functions

Because Thundra originated from the JVM world, we provide a custom runtime to our users with Java. When you use this custom runtime (which can be found in our AWS Lambda Layer), we provide you an option for a fast start-up of your Lambda functions with Java runtime. This allows Java users to improve the start-up performance of their functions up to three times better than with a normal runtime. In order to enable fast-startup, you have two options:

* Configuring Thundra Libraries: After you add Thundra Layer to your function and set the runtime to the custom runtime, all you need to do is set the environment variable with the name `thundra_agent_lambda_jvm_optimizeForFastStartup`to true. More detailed information can be found [here](../node.js/nodejs-integration-options.md).
* Instrumenting from Thundra Web Console: If you already installed our AWS integration, you can instrument your Java functions with the fast start-up mode from the Thundra console with only one click. Thundra will make all the necessary arrangements for you.&#x20;

