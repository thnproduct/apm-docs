# Integration Options for .NET SDK in Containers and VMs

#### Step 1: Add Thundra NuGet package

```bash
dotnet add package Thundra.Agent.Lambda --version $LATEST_VERSION
```

![Latest version](https://img.shields.io/nuget/v/Thundra.Agent.Lambda)

#### Step 2: Add Thundra .NET Middleware &#x20;

After installing the Thundra package, you will need to add Thundra .NET Middleware to your application. Thundra will monitor your application automatically.

{% code title="Startup.cs" %}
```csharp
// ...

// Make sure to load the Thundra Package
using Thundra.Agent.Wrapper.ASP;

namespace SampleApplication
{
    public class Startup
    {
        // ...

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            // ...

            // Thundra .NET Middleware
            app.UseThundra();
            app.UseMvc(routes =>
            {
                // ...
            });
        }
    }
}

```
{% endcode %}

#### Step 3: Add necessary Environment Variables&#x20;

Initially, we recommend setting two environment variables.&#x20;

Set the **`thundra_apiKey`** environment variable to the API key value you got from the Thundra console and the **`thundra_agent_application_name`** to your choice of a name that will show up on the Thundra console.

Below, you'll see some examples of ways to set these variables.

{% code title="Shell" %}
```bash
export thundra_apiKey=<Thundra-ApiKey>
export thundra_agent_application_name=<Your-Application-Name>
```
{% endcode %}

{% code title="Dockerfile" %}
```yaml
ENV thundra_apiKey=<Thundra-ApiKey>
ENV thundra_agent_application_name=<Your-Application-Name>
```
{% endcode %}

{% code title="launchSettings.json" %}
```javascript
{
  // ...
  "profiles": {
    "SampleApplication": {
      // ...
      "environmentVariables": {
        // ...
        "thundra_apiKey": "<Thundra-ApiKey>",
        "thundra_agent_application_name": "<Your-Application-Name>"
      }
    }
  }
}


```
{% endcode %}
