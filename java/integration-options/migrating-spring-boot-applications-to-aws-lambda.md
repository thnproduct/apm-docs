# Migrating Spring Boot Applications to AWS Lambda

### Creating a zipped Spring Boot Fat Jar

To create a bootable jar file for Spring Boot Maven or Gradle, you can follow the instructions here:

* [Packaging Executable for Maven](https://docs.spring.io/spring-boot/docs/current/maven-plugin/reference/htmlsingle/#packaging)
* [Packaging Executable for Gradle](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/htmlsingle/#packaging-executable)

If you are using Docker, you can see the instructions for Lambda Container Images on this page:

{% content-ref url="lambda-container-image.md" %}
[lambda-container-image.md](lambda-container-image.md)
{% endcontent-ref %}

### Adding Thundra APM

We recommend adding Thundra Agent as a Lambda Layer. You can see more about how to do this on this page:

{% content-ref url="lambda-layer.md" %}
[lambda-layer.md](lambda-layer.md)
{% endcontent-ref %}

### Thundra Integration and Necessary Configuration

**Required Steps**

**Step 1:** Set your Lambda's runtime to `Custom Runtime`&#x20;

**Step 2:** Set your Lambda's handler to `io.thundra.agent.lambda.container.springboot2.ContainerHandler`

**Step 3:** Set the `THUNDRA_AGENT_LAMBDA_CONTAINER_SPRINGBOOT_APP_ARTIFACT` environment variable to your application name, e.g. `petclinic.jar`.&#x20;

{% hint style="danger" %}
**Important Note about Step 3**

In this step, you must put your jar file into a separate **zip** file and upload that to AWS that way. Otherwise, if you upload the jar directly, AWS will extract the jar file itself which will break the integration.
{% endhint %}

**Optional Steps**

**Step 4:** Set the `THUNDRA_AGENT_LAMBDA_JVM_OPTIMIZEFORFASTSTARTUP` environment variable to `true` . This is because we want the Lambda runtime JVM process to start with the configured VM arguments that are optimized for a fast start-up. See our [documentation](https://apm.docs.thundra.io/java/integration-options/lambda-layer#good-bye-to-cold-starts-with-fast-startup-mode) on this issue.

**Step 5:** Set the `THUNDRA_AGENT_LAMBDA_REPORT_CLOUDWATCH_ENABLE` environment variable to `true` . This is to report the collected telemetry data over AWS CloudWatch asynchronously. **Beware that when you enable CloudWatch, you also have to set up the CloudWatch adapters.** Make sure to checkout our [documentation](../../performance/zero-overhead-with-asynchronous-monitoring.md) on this issue.
