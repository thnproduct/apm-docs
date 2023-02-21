# Setting up Java SDK with Lambda Layer

{% tabs %}
{% tab title="Lambda Layers" %}
#### **Step 1: Add Thundra API Key**&#x20;

Add the Thundra API key through `THUNDRA_APIKEY` environment variable.

#### ****

#### **Step 2: Add Thundra Layer**

Then, add the Thundra layer.

```
arn:aws:lambda:${region}:269863060030:layer:thundra-lambda-java-layer:${layer-version} 
```

You can use the latest layer version (shown below) instead of the`${layer-version}` above.

<figure><img src="https://api.globadge.com/v1/badgen/aws/lambda/layer/latest-version/us-east-1/269863060030/thundra-lambda-java-layer" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that the region of the ARN is dynamic, so you need to change it accordingly to the region where you deploy your function. So let’s say that you deploy your Lambda function to the Oregon (us-west-2) region. The layer ARN will be:`arn:aws:lambda:us-west-2:269863060030:layer:thundra-lambda-java-layer:${layer-version}`
{% endhint %}

****

**Step 3: Switch Handler**

Set the handler to `io.thundra.agent.lambda.core.handler.ThundraLambdaHandler` **** and set the `THUNDRA_AGENT_LAMBDA_HANDLER` environment variable value to your **original** handler.

## Custom Runtime

Using Thundra's custom Java runtime, you can integrate Thundra to your Java Lambda functions with zero code and configuration changes. Once you add the Thundra layer as described above, all you have to do is change your Lambda function's runtime to `Custom runtime` in the AWS Lambda Console. No `Handler` configuration change is needed.

{% hint style="info" %}
If you are using a Serverless framework, you can set your function’s runtime to "`provided`"  in your `serverless.yml` file. This will set your function to the custom runtime.
{% endhint %}

###

### Good-Bye to Cold Starts with Fast Startup Mode

Java is one of the languages that suffers the most from cold start overhead on AWS Lambda. Until now, with managed Java runtime, it was not feasible to optimize a Java process to start faster.  AWS Lambda does have some optimizations to start processes faster for Java runtime, such as enabling class data-sharing with -`Xshare:on` so that classes are loaded from a shared memory-mapped file in a pre-parsed format. However, there are still some other options to fine-tune a Java application’s startup time.

&#x20;

**Enabling Fast-Startup Mode**

1. Add Thundra Java layer and make handler configuration as described above.
2. Configure runtime
   * **Java 8**
     * Change runtime to `Custom runtime`.
   * **Java 8 (Corretto)** and **Java 11 (Corretto)**
     * Set `AWS_LAMBDA_EXEC_WRAPPER` environment variable to `/opt/thundra_wrapper`
3. Set `THUNDRA_AGENT_LAMBDA_JVM_OPTIMIZEFORFASTSTARTUP`environment variable to `true`.

{% hint style="info" %}
For `Custom runtime`, we use AWS Lambda’s JVM, which is pre-installed on the custom runtime environment and doesn't provide or use any patched/modified JVM.
{% endhint %}



If you’re having trouble with the cold start overhead for your Java-based Lambda function, give it a try and see the effects instantly. \

{% endtab %}
{% endtabs %}
