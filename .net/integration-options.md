# Integration Options for .NET SDK

{% tabs %}
{% tab title="Serverless Framework" %}
### **Serverless Framework**

**Step 1: Add Thundra NuGet package**

The .NET Thundra agent can be easily downloaded with the command shown below:

```bash
dotnet add package Thundra.Agent --version $LATEST_VERSION
```

![Latest Version](https://img.shields.io/nuget/v/Thundra.Agent)

Similarly, you can add the agent via the NuGet Package Manager of your IDE (if available). For example, Visual Studio allows you to add packages through the IDE itself; a simple search for Thundra .NET will allow you to procure and add the correct agent to your project.

**Step 2: Change serverless.yml**

You should replace all of your handlers to “`Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle`” and set the real handler path as a separate environment variable.

{% code title="serverless.yml" %}
```yaml
...

functions:
  FirstFunction:
    handler: Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle
    environment:
      thundra_agent_lambda_handler: <your_handler>
      thundra_apiKey: <your_api_key>

  SecondFunction:
    handler: Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle
    environment:
      thundra_agent_lambda_handler: <your_second_handler>
      thundra_apiKey: <your_api_key>

...
```
{% endcode %}

Using this method, you can connect Thundra to your functions by modifying the configuration files without needing to change your code. The ThundraProxy class will call the actual handler as soon as it receives the request.
{% endtab %}

{% tab title="AWS SAM" %}
### AWS S**AM**

**Step 1: Add the Thundra NuGet Package**

The .NET Thundra agent can be easily downloaded with the command shown below:

```bash
dotnet add package Thundra.Agent --version $LATEST_VERSION
```

![Latest Version](https://img.shields.io/nuget/v/Thundra.Agent)

Similarly, you can add the agent via the NuGet Package Manager of your IDE (if available). For example, Visual Studio allows you to add packages through the IDE itself; a simple search for Thundra .NET will allow you to procure and add the correct agent to your project.

**Step 2: Change template.yaml**

You should replace all of your handlers to “`Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle`” and set the real handler path as a separate environment variable.

{% code title="template.yaml" %}
```yaml
....

    FirstFunction:
        Type: AWS::Serverless::Function 
        Properties:
           Handler: Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle
           Environment:
               Variables:
                   thundra_agent_lambda_handler: <your_handler>
                   thundra_apiKey: <your_api_key>

    SecondFunction:
        Type: AWS::Serverless::Function 
        Properties:
           Handler: Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle
           Environment:
               Variables:
                   thundra_agent_lambda_handler: <your_second_handler> 
                   thundra_apiKey: <your_api_key>

...
```
{% endcode %}

Using this method, you can connect Thundra to your functions by modifying the configuration files without needing to change your code. The ThundraProxy class will call the actual handler as soon as it receives the request.
{% endtab %}

{% tab title="AWS CDK" %}
### AWS CDK

**Step 1: Add the Thundra NuGet Package to Your Lambda**&#x20;

The .NET Thundra agent can be easily downloaded with the command shown below:

```bash
dotnet add package Thundra.Agent --version $LATEST_VERSION
```

![Latest Version](https://img.shields.io/nuget/v/Thundra.Agent)

Similarly, you can add the agent via the NuGet Package Manager of your IDE (if available). For example, Visual Studio allows you to add packages through the IDE itself; a simple search for Thundra .NET will allow you to procure and add the correct agent to your project.

#### **Step 2: Apply Configuration Changes to Your Function Properties**

* Add the `thundra_apiKey` environment variable with your Thundra API key.

```csharp
using Amazon.CDK;
using Amazon.CDK.AWS.Lambda;
using System.Collections.Generic;

namespace YourNameSpace
{
    public class YourConstructClass : Construct
    {   
        public YourConstructClass(Construct scope, string id) : base(scope, id)
        {
            var thundraApiKey = <your_api_key>;
            
            var handler = new Function(this, "YourHandler", new FunctionProps
            {
                ..., // other function properties
                Environment = new Dictionary<string, string>
                {
                    ..., // other environment variables
                    ["thundra_apiKey"] = thundraApiKey
                }
            });
        }
    }
}
```

*   Change the `Handler` of the function to

    ```
    Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle
    ```

    and add the `thundra_agent_lambda_handler` environment variable to the actual handler of your function.

```csharp
using Amazon.CDK;
using Amazon.CDK.AWS.Lambda;
using System.Collections.Generic;

namespace YourNameSpace
{
    public class YourConstructClass : Construct
    {
        public YourConstructClass(Construct scope, string id) : base(scope, id)
        {
            var thundraApiKey = <your_api_key>;
            
            var handler = new Function(this, "YourHandler", new FunctionProps
            {
                ..., // other function properties
                Handler = "Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle",
                Environment = new Dictionary<string, string>
                {
                    ..., // other environment variables
                    ["thundra_apiKey"] = thundraApiKey,
                    ["thundra_agent_lambda_handler"] = "Your.Assembly::Your.Type::YourMethod"
                }
            });
        }
    }
}
```

An example configuration:

```csharp
using Amazon.CDK;
using Amazon.CDK.AWS.Lambda;
using System.Collections.Generic;

namespace YourNameSpace
{
    public class YourConstructClass : Construct
    {
        public YourConstructClass(Construct scope, string id) : base(scope, id)
        {
            var thundraApiKey = <your_api_key>;
            
            var handler = new Function(this, "WidgetHandler", new FunctionProps
            {
                Runtime = Runtime.DOTNET_CORE_3_1,
                Code = Code.FromAsset("path/to/function/package"),
                Handler = "Thundra.Agent::Thundra.Agent.Lambda.Core.ThundraProxy::Handle",
                Environment = new Dictionary<string, string>
                {
                    ["thundra_apiKey"] = thundraApiKey,
                    ["thundra_agent_lambda_handler"] = "Your.Assembly::Your.Type::YourMethod"
                }
            });
        }
    }
}
```

#### ****

#### **Step 2: Build and Deploy**

```bash
dotnet build src && cdk deploy
```

#### Step 3: Invoke Your Function

Now you can invoke your Lambda function and see the details of your invocation in the Thundra console!
{% endtab %}

{% tab title="Programmatic" %}
**Step 1: Add the Thundra NuGet Package**

The .NET Thundra agent can be easily downloaded with the command shown below:

```bash
dotnet add package Thundra.Agent --version $LATEST_VERSION
```

![Latest Version](https://img.shields.io/nuget/v/Thundra.Agent)

Similarly, you can add the agent via the NuGet Package Manager of your IDE (if available). For example, Visual Studio allows you to add packages through the IDE itself; a simple search for Thundra .NET will allow you to procure and add the correct agent to your project.

**Step 2: Change the Handler's Code**

Extend Thundra's `LambdaRequestHandler` within your handler class and then override the `DoHandleRequest` method, which encapsulates your handler code. `LambdaRequestHandler` takes two generic classes as parameters: the type of request object and the type of response object.

{% code title="Function.cs" %}
```csharp
using Amazon.Lambda.Core;
using Thundra.Agent.Lambda.Core;

namespace ThundraSample
{
    public class Function : LambdaRequestHandler<string, string>
    {

        /// <summary>
        /// Handler class needs to extend `LambdaRequestHandler< Request, Response >`
        /// Please write all code within the DoHandleRequest method
        /// </summary>
        /// <param name="request"></param>
        /// <param name="context"></param>
        /// <returns>/Greeting Message/</returns>
        public override string DoHandleRequest(string request, ILambdaContext context)
        {
            return "Hello Thundra";
        }
    }
}
```
{% endcode %}

**Step 3: Change Your Handler to Thundra's HandleRequest**

Next, you should change your handler to `<MyAssembly>::<MyNamespace>.<MyClass>::HandleRequest`.

It is a common mistake to utilize the hook method `DoHandleRequest` as the handler. In order to initialize Thundra, you must change your handler to `HandleRequest`, which resides in the base class (`LambdaRequestHandler`).&#x20;
{% endtab %}
{% endtabs %}











****
