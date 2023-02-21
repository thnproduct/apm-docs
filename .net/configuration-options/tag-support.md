# Tagging in .NET SDK

Thundra's .NET agent enables you to add custom tags to your invocations or applications, which will help you better filter your data.

## Tagging Invocations

You can add custom tags to your invocation with the help of static methods of Thundra.Agent.Lambda.Core.InvocationSupport object. These are the methods you can find in the InvocationSupport class.

{% hint style="info" %}
#### Searchable Invocation Tags

For now, only primitive `String`, `Number`, and `Boolean` tag values can be searched. Other complex tag values (`List`, `Set`, `Map`, `Object`, etc.) cannot be searched, but can be seen in an invocation’s details.
{% endhint %}

Shown below is the`InvocationSupport`'s programmatic tag API:

{% code title="InvocationSupport.cs" %}
```csharp
public static string InvocationId;
public static Dictionary<string, object> GetTags();
public static void SetTag(string key, object value); 
public static object GetTag(string key);
public static void SetError(Exception error);
public static Exception GetError();
public static void ClearTag(string key);
```
{% endcode %}

The example below shows an usage of the `InvocationSupport` class:

```csharp
using Thundra.Agent.Lambda.Core.InvocationSupport;

...

Console.WriteLine(InvocationSupport.InvocationId);

InvocationSupport.SetTag("user-id", 1980);
InvocationSupport.SetTag("user-name", "Dudley Dursley");
InvocationSupport.SetTag("is-muggle", true);
InvocationSupport.SetError(new UserException("Muggles not allowed"));
```

## Set error to manually created OpenTracing Span

`Thundra.Agent.Lambda.Core.SpanSupport` tags a custom span that is created via OpenTracing API as erroneous. To learn more about how to create custom spans in order to further your monitoring experience, visit the [Enrich Tracing](../enrich-tracing.md) page.

```csharp
using (IScope updateEntityScope = 
       GlobalTracer.Instance.BuildSpan("UpdateEntity")
       .StartActive(finishSpanOnDispose: true))
{
  try 
  {
    DoRiskyThings();
  }
  catch(Exception e)
  {
    SpanSupport.SetError(updateEntityScope.Span, e);
    throw e;
  }
}
```
