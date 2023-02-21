# Setting up Java SDK with AWS CDK

{% tabs %}
{% tab title="AWS CDK" %}
#### **Step 1: Add Thundra API Key**

Add the `THUNDRA_APIKEY` environment variable along with your Thundra API key.

```java
import software.amazon.awscdk.core.Construct;
import software.amazon.awscdk.services.lambda.Function

public class YourConstructClass extends Construct {

    String thundraApiKey = <YOUR THUNDRA API KEY>;
    
    public YourConstructClass(Construct scope, String id) {
      
        Function function = Function.Builder.create(this, "YourFunction")
            ... // other function properties
            .environment(new HashMap<String, String>() {{
                ... // other environment variables
                put("THUNDRA_APIKEY", thundraApiKey);
            }})
            .build();
    }
                    
}
```



#### **Step 2: Add Thundra Layer**

Define the Thundra layer and add it to your function properties.

{% hint style="info" %}
Latest version of Thundra's Java layer: ![](https://img.shields.io/endpoint.svg?url=https://thundra-layer-java-d136so2ggl5e.runkit.sh)
{% endhint %}

```java
import software.amazon.awscdk.core.Construct;
import software.amazon.awscdk.services.lambda.Function
import software.amazon.awscdk.services.lambda.ILayerVersion;
import software.amazon.awscdk.services.lambda.LayerVersion;

public class YourConstructClass extends Construct {

    String thundraApiKey = <YOUR THUNDRA API KEY>;
    String thundraAWSAccountNo = "269863060030";
    String thundraJavaLayerVersion = "65"; // or any other version;
    ILayerVersion thundraLayer = LayerVersion.fromLayerVersionArn(
        this,
        null,
        String.format(
            "arn:aws:lambda:%s:%s:layer:thundra-lambda-java-layer:%s",
            Aws.REGION, thundraAWSAccountNo, thundraJavaLayerVersion
        )
    );

    public YourConstructClass(Construct scope, String id) {
        Function function = Function.Builder.create(this, "YourFunction")
            ... // other function properties
            .environment(new HashMap<String, String>() {{
                ... // other environment variables
                put("THUNDRA_APIKEY", thundraApiKey);
            }})
            .layers(Arrays.asList(
                thundraLayer,
                ... // other layers
            ))
            .build();
    }
             
}
```

{% hint style="info" %}
`Aws.REGION` is a pseudo parameter which is bootstrapped from your stack's environment configuration.
{% endhint %}



#### **Step 3: Switch Handler**

Change the `handler` of functions to be wrapped to `io.thundra.agent.lambda.core.handler.ThundraLambdaHandler`

and add the `THUNDRA_AGENT_LAMBDA_HANDLER`  environment variable with your original handler.

```java
import software.amazon.awscdk.core.Construct;
import software.amazon.awscdk.services.lambda.Function
import software.amazon.awscdk.services.lambda.ILayerVersion;
import software.amazon.awscdk.services.lambda.LayerVersion;
import software.amazon.awscdk.core.Aws;

public class YourConstructClass extends Construct {

    String thundraApiKey = <YOUR THUNDRA API KEY>;
    String thundraAWSAccountNo = "269863060030";
    String thundraJavaLayerVersion = "65"; // or any other version;
    ILayerVersion thundraLayer = LayerVersion.fromLayerVersionArn(
        this,
        null,
        String.format(
            "arn:aws:lambda:%s:%s:layer:thundra-lambda-java-layer:%s",
            Aws.REGION, thundraAWSAccountNo, thundraJavaLayerVersion
        )
    );

    public YourConstructClass(Construct scope, String id) {
        Function function = Function.Builder.create(this, "MyFunction")
            ... // other function properties
            .handler("io.thundra.agent.lambda.core.handler.ThundraLambdaHandler")
            .environment(new HashMap<String, String>() {{
                ... // other environment variables
                put("THUNDRA_APIKEY", thundraApiKey);
                put("THUNDRA_AGENT_LAMBDA_HANDLER", "yourdomain.yourpackage.yourhandler");
            }})
             .layers(Arrays.asList(
                thundraLayer,
                ... // other layers
            ))
            .build();
    }

}
```

An example configuration:

```java
import software.amazon.awscdk.core.Construct;
import software.amazon.awscdk.services.lambda.Function
import software.amazon.awscdk.services.lambda.ILayerVersion;
import software.amazon.awscdk.services.lambda.LayerVersion;
import software.amazon.awscdk.services.lambda.Runtime;
import software.amazon.awscdk.core.Aws;

public class YourConstructClass extends Construct {

    String thundraApiKey = "<YOUR THUNDRA API KEY>";
    String thundraAWSAccountNo = "269863060030";
    String thundraJavaLayerVersion = "65" // or any other version;
    ILayerVersion thundraLayer = LayerVersion.fromLayerVersionArn(
        this,
        null,
        String.format(
            "arn:aws:lambda:%s:%s:layer:thundra-lambda-java-layer:%s",
            Aws.REGION, thundraAWSAccountNo, thundraJavaLayerVersion
        )
    );

    public YourConstructClass(Construct scope, String id) {
        Function function = Function.Builder.create(this, "YourFunction")
            .runtime(Runtime.JAVA_8)
            .code(Code.fromAsset("/path/to/your/function/package"))
            .handler("io.thundra.agent.lambda.core.handler.ThundraLambdaHandler")
            .environment(new HashMap<String, String>() {{
                put("THUNDRA_APIKEY", thundraApiKey);
                put("THUNDRA_AGENT_LAMBDA_HANDLER", "yourdomain.yourpackage.yourhandler");
            }})
            .layers(Collections.singletonList(thundraLayer))
            .build();
    }
    
}
```

****
{% endtab %}
{% endtabs %}
