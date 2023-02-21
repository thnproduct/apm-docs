# Enrich Tracing in .NET

### Manual Instrumentation with OpenTracing

Thundra’s agents are fully compliant with OpenTracing API. This means that you can access Thundra’s tracer via OpenTracing’s GlobalTracer instance and build new spans as well as add business information to increase observability. The Thundra agent creates the root span automatically, which allows you to track request times. Additionally, you can create individual spans for the parts of your function that you want to monitor (e.g., database requests, external web service calls, or business transactions). See the following sample code:

```csharp
using System;
using System.Linq;
using System.Threading;
using Amazon.Lambda.Core;
using Microsoft.Extensions.Logging;
using Thundra.Agent.Lambda.Core;
using Thundra.Agent.Log.AspNetCore;
using OpenTracing.Util;
using OpenTracing;

namespace blog_opentracing
{
    public class Function : LambdaRequestHandler<string, string>
    {
       public override string DoHandleRequest(string request, ILambdaContext context)
       {
           
           var loggerFactory = new LoggerFactory().AddThundraProvider();
           var logger = loggerFactory.CreateLogger<Function>();

           Console.WriteLine("Hello OpenTracing.");

           ValidateEntity();
           using (IScope updateEntityScope = GlobalTracer.Instance.BuildSpan("UpdateEntity").StartActive(finishSpanOnDispose: true))
           {
               updateEntityScope.Span.SetTag("updatedEntityID", 1);
               updateEntityScope.Span.SetTag("entitySize","1KB");
               using (IScope updateDatabaseScope = GlobalTracer.Instance.BuildSpan("UpdateDatabase").StartActive(finishSpanOnDispose: true))
               {
                   UpdateDatabase();
               }
           }

           return request?.ToUpper();
       }

       private void ValidateEntity() {
           IScope validationScope = GlobalTracer.Instance.BuildSpan("Validation").StartActive(finishSpanOnDispose: true);
           //some validations...
           validationScope.Dispose();
       }

       private void UpdateDatabase()
       {
           //some database calls...
       }
   }
}
```

The resulting trace chart for this function, which you will see in the Thundra web console, will be similar to the following image.

![](<../.gitbook/assets/Screen Shot 2019-10-07 at 10.03.27.png>)

As you can see, there is a root span that Thundra automatically created in addition to other spans which are the children of the root span.

## &#x20;
