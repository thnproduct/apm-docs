# Setting up Java SDK with AWS SAM

{% tabs %}
{% tab title="AWS SAM" %}
#### **Step 1: Add Thundra API Key**

Add the `THUNDRA_APIKEY` environment variable along with your Thundra API key in the `template.yml`.

{% code title="template.yml" %}
```yaml
Globals:
  Function:
	...
    Environment:
      Variables:
          THUNDRA_APIKEY: <YOUR THUNDRA API KEY>
```
{% endcode %}



#### **Step 2: Add Thundra Layer**

Add the Thundra layer to “`Layers`” in the `Globals` section in the `template.yml`. The `ThundraAWSAccountNo` and `ThundraJavaLayerVersion` parameters are defined under the `Parameters` section in the following configuration:

Latest version of the Thundra Java layer:

<figure><img src="https://api.globadge.com/v1/badgen/aws/lambda/layer/latest-version/us-east-1/269863060030/thundra-lambda-java-layer" alt=""><figcaption></figcaption></figure>



```yaml
Parameters:
  ThundraAWSAccountNo:
    Type: Number
    Default: 269863060030

  ThundraJavaLayerVersion:
    Type: Number
    Default: 71 # Or use any other version

Globals:
  	...
    Layers:
    	- !Sub arn:aws:lambda:${AWS::Region}:${ThundraAWSAccountNo}:layer:thundra-lambda-java-layer:${ThundraJavaLayerVersion}

	...

```



#### **Step 3: Switch Handler**

Change the `Handler` of functions to be wrapped to `io.thundra.agent.lambda.core.handler.ThundraLambdaHandler`.

 

* If you want to wrap all of the functions in your SAM configuration file, you can set the `Handler` in the `Globals` section.

```yaml
Globals:
  Function:
    ...

    Handler: io.thundra.agent.lambda.core.handler.ThundraLambdaHandler

```



Then, for each wrapped function, add the `THUNDRA_AGENT_LAMBDA_HANDLER` environment variable with the full class-name (package and class name) of your function's original handler.

```yaml
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Environment:
          Variables:
              THUNDRA_AGENT_LAMBDA_HANDLER: com.mycompany.MyHandler

```

An example configuration:

```yaml
Parameters:
  ThundraAWSAccountNo:
    Type: Number
    Default: 269863060030

  ThundraJavaLayerVersion:
    Type: Number
    Default: 65 # Or use any other version

Globals:
  Function:
    Runtime: java8
    Timeout: 30
    Handler: io.thundra.agent.lambda.core.handler.ThundraLambdaHandler
    Layers:
    	- !Sub arn:aws:lambda:${AWS::Region}:${ThundraAWSAccountNo}:layer:thundra-lambda-java-layer:${ThundraJavaLayerVersion}
    Environment:
      Variables:
          THUNDRA_APIKEY: <YOUR THUNDRA API KEY>

Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Environment:
          Variables:
              THUNDRA_AGENT_LAMBDA_HANDLER: com.mycompany.MyHandler

```
{% endtab %}
{% endtabs %}
