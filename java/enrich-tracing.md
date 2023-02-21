# Enrich Tracing in Java

### Manual Instrumentation with Open Tracing

Automated instrumentation works well, but sometimes it’s not always enough to meet your needs, so you may want to instrument on your own. Through the global `ThundraTracer`, which is retrieved from `io.thundra.agent.trace.TraceSupport`, you can create your own custom spans. In the following example, we show how to create a custom span to instrument user operation.

{% code title="UserService" %}
```java

...

import io.thundra.agent.trace.TraceSupport;
import io.thundra.agent.trace.ThundraTracer;
import io.thundra.agent.trace.span.ThundraSpan;

...

public class UserService {

    private final ThundraTracer tracer = TraceSupport.getTracer();
    
    ...

    public User get(String id) {
        ThundraSpan span = tracer.buildSpan("user.get").startActive(true).span();
        try {
            if (user != null) {
                span.setTag("hit", true);
                span.setTag("get.user", user);
            } else {
                span.setTag("hit", false);
                ...
            }
        } finally {
            span.finish();
        }

        return user;
    }

    ...

}
```
{% endcode %}

### Automated Instrumentation

There are two ways to configure audit (trace) support:

* **By Annotation**: Classes and methods that you want to audit can be specified and configured with the @io.thundra.agent.trace.instrument.config.Traceable annotation.
* **By Environment Variable**: Classes and methods that you want to trace can be specified and configured with the thundra\_agent\_trace\_instrument\_traceableConfig environment variable.

**Note**: Thundra doesn’t support tracing for a Lambda by instrumenting the handler (it is already traced by wrapping the invocation). So don’t use it in the manner seen in the code snippet below.

#### Specify classes/methods to be traced

**By Annotation**

Classes and methods you want to trace can be specified with the `@Traceable` annotation – they must be marked with this annotation. The class level `@Traceable` annotation is mandatory; if not marked with the annotation, classes won’t be checked or traced. Additionally, methods can be marked with this annotation as well.

* If the annotation is used on the class, all public methods of the class are traced by default.&#x20;

{% code title="UserService.java" %}
```java
...

import io.thundra.agent.trace.instrument.config.Traceable;

...

@Traceable
public class UserService {

    ...
    
    public User get(String id) {
        ...
    }    

}
```
{% endcode %}

In the example above, the public `get` method is traced automatically.

* If the annotation is used on the method, the specified method is also traced.

{% code title="UserService.java" %}
```java
...

import io.thundra.agent.trace.instrument.config.Traceable;

...

@Traceable
public class UserService {

    ...

    @Traceable
    private User getFromCache(String id) {
        ...
    }

    @Traceable
    private User getFromRepository(String id) {
        ...
    }

    ...
}
```
{% endcode %}

In this example,  the `getFromCache` and `getFromRepository` methods are traced in addition to the `get` method.

If you want to explicitly specify a method to be traced, configure the class level @Traceable annotation by setting the `justMarker` property and annotate the desired method with the method level `@Traceable` annotation.

{% code title="UserService.java" %}
```java
...

import io.thundra.agent.trace.instrument.config.Traceable;

...

@Traceable(justMarker=true)
public class UserService {

    @Traceable
    public User get(String id) {
        ...
    }

    ...

}
```
{% endcode %}

In this example, only the `get` method is traced because the class level @Traceable annotation is marked and only the `get` method is annotated.

**By Environment Variable**

Environment variable names must be in the `thundra_agent_trace_instrument_traceableConfig*` format, meaning a name must start with the `thundra_agent_trace_instrument_traceableConfig`. For example, `thundra_agent_trace_instrument_traceableConfig`, `thundra_agent_trace_instrument_traceableConfig1`, `thundra_audit_auditableDefx`, etc.

Environment variable values must be in the `<class-def>.<method-def>[propName1=propValue1,propName2=propValue2,...]` format, where property definitions are optional. The asterisk character (\*) in the `<class-def>` and `<method-def>` is supported. For example:

* `get` method in the `com.mycompany.UserService` class: `com.mycompany.UserService.get`
* All methods start with `validate` in the class `com.mycompany.UserService`: `com.mycompany.UserService.validate*`
* All methods in the `com.mycompany.UserService` class: `com.mycompany.UserService.*`
* All methods in the `com.mycompany` package: `com.mycompany.*.*`

{% hint style="info" %}
`thundra_agent_trace_instrument_traceableConfig` environment variables can be ordered. If a definition’s order is lower than others, it is checked before those with a higher order. So a `thundra_agent_trace_instrument_traceableConfig` definition can override another definition that has a higher order. Default order is `0`. There are two ways to configure order of an `thundra_agent_trace_instrument_traceableConfig` environment variable:

* By order property. For example, `com.mycompany.UserService.*[...,...,order=1]` means that the order of the definition is `1`.
* By number post-fix after the `thundra_agent_trace_instrument_traceableConfig` environment variable name. For example, `thundra_agent_trace_instrument_traceableConfig1` means that the order of the definition is `1`.
{% endhint %}

Additionally, by using the `thundra_agent_trace_instrument_traceablePrefixes` environment variable, you can narrow down the scope of classes to be checked in order to reduce instrumentation overhead (and reduce cold-start overhead as well). Multiple prefixes can be specified by splitting them with a comma (`,`).

This configuration is strongly recommended to start up Lambda applications faster. For example, you can set the `thundra_agent_trace_instrument_traceablePrefixes` environment variable to `io.thundra.lambda.demo` and thus only check classes from the `io.thundra.lambda.demo` package.

#### Method Modifiers

Should you want to trace private methods as well, you can specify method modifiers.

**Configuration via Annotations**

The `methodModifiers` attribute of the `@Traceable` annotation can be configured to specify modifiers of methods to be traced. In this instance, modifiers are the flags defined in the `java.lang.reflect.Modifier` class. The `ANY_METHOD_MODIFIER` constant in the `io.thundra.agent.trace.instrument.config.TraceableConfig` can be used to trace any defined method in a class.

For example, if you want to trace all public and private methods in the `UserService` class:

{% code title="UserService.java" %}
```java
...

import io.thundra.agent.trace.instrument.config.Traceable;
import java.lang.reflect.Modifier;

...

@Traceable( 
    methodModifiers = { Modifier.PUBLIC, Modifier.PRIVATE } 
)
public class UserService {

    ...

}
```
{% endcode %}

**By Environment Variable**

To configure method modifiers through an environment variable, the `methodModifiers` property of the `thundra_agent_trace_instrument_traceableConfig` environment variable can be configured as a hexadecimal format definition of modifiers from the `java.lang.reflect.Modifier` class, as shown below:

* `0x00000001` means public methods.
* `0x00000002` means private methods.
* `0x00000004` means protected methods.
* `0x00000008` means static methods.
* `0x00000001 | 0x00000002` means private or public methods. As shown here, multiple modifiers can be combined with the | character.
* `0x70000000` means any method.

Note that, as shown in the examples above, multiple modifiers can be combined with the `|` character and this means logical `OR`.

In our example, if we want to trace all public and private methods in the `UserService` class over environment variables, the `thundra_agent_trace_instrument_traceableConfig` environment variable can be specified as `io.thundra.lambda.demo.service.UserService.*[...,...,methodModifiers=0x00000001|0x00000002]`

Trace arguments can be described as names, types, and values of variables within your Lambda functions. Due to the black box nature of serverless, monitoring these parameters is difficult yet necessary when debugging and testing your code. Hence, one of the aims of Thundra's trace capabilities is to ensure ease of monitoring of your arguments, which is done by configuring your trace per your needs. Enabling argument support can be done either by annotation or environment variables.

**Configuration via Annotations**

The `traceArguments` attribute of the `@Traceable` annotation can be set to `true` for tracing method arguments. Additionally, the following properties can be configured for more detailed argument tracing:

* By default, an argument name is generated from argument order, such as `arg-0`, `arg-1`, etc. The `traceArgumentNames` attribute can be set to true, which will extract argument names if a local variable table of the associated method is available in the owner class’s bytecode.
* By default, argument value is generated from the `toString` method of the argument instance. If you want to serialize the argument value in a structured format, such as JSON, you can set the `serializeArgumentsAsJson` attribute to `true`.

```java
...

import io.thundra.agent.trace.instrument.config.Traceable;

...

@Traceable( 
    ...,
    traceArguments = true, 
    tracetArgumentNames = true, 
    serializeArgumentsAsJson = true 
)
public class UserService {

    ...

}
```

**Configuration via Environment Variables**

To configure tracing argument behavior with environment variables, the `traceArguments` property of the `thundra_agent_trace_instrument_traceableConfig` environment variable can be set to `true`.

Similarly:

* Tracing the behavior of argument names can be configured by setting the `traceArgumentNames` property of the `thundra_agent_trace_instrument_traceableConfig` environment variable to `true`.
* Serializing arguments’ values as JSON can be configured by setting `serializeArgumentsAsJson` to `true`.

If you want to trace arguments (names, types, and values) of monitored methods in, for example, the `UserService` class over environment variables, the `thundra_agent_trace_instrument_traceableConfig` environment variable can be specified as `io.thundra.lambda.demo.service.UserService.*[...,...,traceArguments=true,traceArgumentNames=true,serializeArgumentsAsJson=true].`

#### Trace Return Values

Overall, Thundra's Java trace support allows you to perform method-level tracing. However, that may not always be sufficient, as this does not provide information about the return values of your Lambda functions. Utilizing Thundra's Java agent allows you to trace these return values upon your function’s invocations. By default, the feature is disabled, but can be configured by either annotations environment variables.

**Configuration via Annotations**

The `traceReturnValue` attribute of the `@Traceable` annotation can be set to `true` in order to trace return values. By default, the return value is generated from the `toString` method of the return value instance. If you want to serialize the return value in a structured format, such as JSON, you can set the `serializeReturnValueAsJson` attribute to `true`.

{% code title="UserService.java" %}
```java
...


import io.thundra.agent.trace.instrument.config.Traceable;

...

@Traceable( 
    ...,
    traceReturnValue = true, 
    serializeReturnValueAsJson = true 
)
public class UserService {

    ...

}
```
{% endcode %}

**Configuration via Environment Variables**

To configure tracing the behavior of return values through environment variables, the `traceReturnValue` property of the `thundra_agent_trace_instrument_traceableConfig` environment variable can be set to `true`. In a similar way, serializing return values as JSON can be configured by setting `serializeReturnValueAsJson` to `true`.

For example, if you want to trace return values (types and values) of monitored methods in the `UserService` class over environment variables, the `thundra_agent_trace_instrument_traceableConfig` environment variable can be specified as `io.thundra.lambda.demo.service.UserService.*[...,...,traceReturnValue=true,serializeReturnValueAsJson=true].`

#### Trace Errors

Thundra's trace support allows you to monitor error causes in each generated span, which in turn represents each part of your function being monitored. Monitoring any errors thrown is an integral part of testing and implementing your functions. Moreover, as the result of edge cases or the failure of external services, several errors may occur in your Lambda functions that would generally not occur during implementation. Thus, tracing errors is an essential parameter, and can be monitored using Thundra's trace support. This feature is disabled by default, but can be enabled by either annotations or environment variables.

**Configuration via Annotations**

To trace errors thrown from methods, the `traceError` attribute of the `@Traceable` annotation can be set to `true`.

{% code title="UserService.java" %}
```java
...

import io.thundra.agent.trace.instrument.config.Traceable;

...
@Traceable( 
    ...,
    traceError = true
)
public class UserService {

    ...

}
```
{% endcode %}

**Configuration via Environment Variables**

To configure tracing error behavior through environment variables, the `traceError` property of the `thundra_agent_trace_instrument_traceableConfig` environment variable can be set to `true`.

For example, if you want to trace errors thrown from monitored methods in the `UserService` class over environment variables, the `thundra_agent_trace_instrument_traceableConfig` environment variable can be specified as `io.thundra.lambda.demo.service.UserService.*[...,...,traceError=true].`

#### Offline Debugging

Offline debugging is supported by Thundra's Java agent and it allows you to monitor your Lambda functions down to each line of code. That means you can see how your functions behave at every level, giving you in-depth monitoring capabilities along with micro-tracing capability even inside the method. In addition to seeing the duration of each line, code execution flow in the method can be traced according to which line was executed after another line. Offline debugging behavior is disabled by default,  but can be enabled by using annotations.

{% hint style="info" %}
As offline debugging requires source code, the `thundra-agent-trace-instrument-metadatagen` module needs to be in the dependencies/classpath during compilation. It includes the Java Annotation processor and generates metadata about classes to be used for tracing at runtime, like offline debugging.

Note that as this module doesn't require runtime, it can be filtered from a final artifact (e.g., by the `provided` scope for Maven).

```markup
<dependency>
    <groupId>io.thundra.agent</groupId>
    <artifactId>thundra-agent-trace-instrument-metadatagen</artifactId>
    <scope>provided</scope>
    <version>${thundra.version}</version>
</dependency>
```
{% endhint %}

**Configuration via Annotations**

The `traceLineByLine` attribute of the `@Traceable` annotation can be set to `true` for debugging methods offline.

{% code title="UserService.java" %}
```java
...


import io.thundra.agent.trace.instrument.config.Traceable;

...

@Traceable( 
    ...,
    traceLineByLine = true
)
public class UserService {

    ...

}
```
{% endcode %}

### API Gateway Custom Authorizer Support

#### `TOKEN` Authorizer

Thundra provides the `io.thundra.agent.lambda.core.auth.TokenAuthorizerContext` class as the input type for your custom authorizer. It provides all the required properties, such as `authorizationToken` and `methodArn`. To see your custom authorizer in a distributed trace map, you need to use this `TokenAuthorizerContext` class as the input type for your custom authorizer handler.

#### `REQUEST` Authorizer

Thundra provides the `io.thundra.agent.lambda.core.auth.RequestAuthorizerContext` class as the input type for your custom authorizer. It provides all the required properties, such as `methodArn`, `resource`, `path`, `httpMethod`, `header`, `queryStringParameters`, `pathParameters`, `stageVariables`, `requestContext`, etc.. To see your custom authorizer in a distributed trace map, you need to use this `RequestAuthorizerContext` class as the input type for your custom authorizer handler.

###

