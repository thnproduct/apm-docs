# Setting up Thundra Java Library manually

{% tabs %}
{% tab title="Thundra Java Library" %}
#### **Step 1: Add Thundra Repo**

Add Thundra repo (`repo.thundra.io`) to your repositories:

```markup
<repositories>
    <repository>
        <id>thundra-releases</id>
        <name>Thundra Releases</name>
        <url>https://repo.thundra.io/content/repositories/thundra-releases</url>
    </repository>
</repositories>
```

#### ****

#### **Step 2: Install Thundra Java Library**&#x20;

Add Thundra dependency:

```markup
<dependency>
    <groupId>io.thundra.agent</groupId>
    <artifactId>thundra-agent-lambda-all</artifactId>
    <type>pom</type>
    <version>${thundra.version}</version>
</dependency>
```

You can use the version shown below instead of `${thundra.version}`.

![](https://img.shields.io/nexus/thundra-releases/io.thundra.agent/thundra-agent-lambda-core?server=http%3A%2F%2Frepo.thundra.io)

#### **Step 3: Bundle and Deploy Your Function to AWS Lambda**&#x20;

Bundle all your Java module files along with additional 3rd party dependencies and Thundra library, and then upload it.

{% hint style="warning" %}
While you are building your artifact along with Thundra library, be sure that Thundra service files (under `META-INF/services`) are merged properly.

For example if you are using **Maven Shade** plugin, be sure that you have`ServicesResourceTransformer`transformer configured:

{% code title="pom.xml" %}
```markup
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>${maven.shade.plugin.version}</version>
<configuration>
    <createDependencyReducedPom>false</createDependencyReducedPom>
    <transformers>
        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
    </transformers>
</configuration>
```
{% endcode %}
{% endhint %}

#### ****

#### **Step 4: Add Thundra API Key**&#x20;

Add the Thundra API key through `THUNDRA_APIKEY` environment variable

#### ****

#### **Step 5: Switch Handler**

Set the handler to `io.thundra.agent.lambda.core.handler.ThundraLambdaHandler` **** and set the `THUNDRA_AGENT_LAMBDA_HANDLER` environment variable value to your **original** handler.
{% endtab %}
{% endtabs %}
