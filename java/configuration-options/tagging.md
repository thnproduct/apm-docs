# Tagging in Java SDK

With Thundra's Java agent, you can add custom tags to your invocations or your applications, which will help you better filter the data. Invocation tags are added only to `Invocation` data type; application tags are global and are automatically added to `Invocation`, `Metric`, `Trace`, `Span`, and `Log` data.

{% hint style="warning" %}
For now, only primitive `String`, `Number` and `Boolean` typed tag values can be searched. Other complex typed (`List`, `Set`, `Map`, `Object`, ...) tags values cannot be searched but can be seen in the invocation or application details.
{% endhint %}

## Adding Invocation Tags

You can add custom tags to your invocation with static methods of the `io.thundra.agent.lambda.core.invocation.InvocationSupport` object. These are the methods you can find in the `InvocationSupport` class.

Here is the `InvocationSupport'`s programmatic tag API:

{% code title="Tag Support API" %}
```java
public static <T> T getTag(String name);
public static Map<String, Object> getTags();
public static void setTag(String name, Object value);
public static void setTags(Map<String, Object> tags);
public static <T> T removeTag(String name);
public static void removeTags();
```
{% endcode %}

The example below shows how to tag a Lambda invocation.

{% code title="Adding Tags" %}
```java
import io.thundra.agent.lambda.core.invocation.InvocationSupport;

...
  
InvocationSupport.setTag('string-tag', 'thundra');
InvocationSupport.setTag('numeric-tag', 123);
InvocationSupport.setTag('boolean-tag', true);
InvocationSupport.setTag('object-tag', new User(username, email, ...));
  
...
```
{% endcode %}

## Adding Application Tags

You can add application tags via environment variables. All application tags which start with `thundra_agent_application_tag_` are parsed by Thundra's Java agent. The text after the `thundra_agent_application_tag_` prefix is used as the key of the tag, and the value of the environment variable becomes the value of the tag. For example, `thundra_agent_application_tag_floatField` with the value `12.123` will add an application tag to all data with the key `floatField` and the value `12.123`.
