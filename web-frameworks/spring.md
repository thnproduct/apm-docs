# Spring

### Step 1: Download Thundra Agent

Download the Thundra agent [here](https://repo.thundra.io/service/local/artifact/maven/redirect?r=thundra-releases\&g=io.thundra.agent\&a=thundra-agent-bootstrap\&v=LATEST).

### Step 2: Start Your Application

Start your Java application with the Thundra agent jar by `-javaagent` VM argument.

```
java -javaagent:<path-to-thundra-agent> -jar <your-app-jar> ...
```

### Step 3: Configure the Dockerfile

The following example shows how the application is configured with Dockerfile:

```
FROM openjdk:8
RUN mkdir -p /app
ADD target/thundra-container-demo-0.1.0.jar /app/thundra-container-demo.jar
ADD thundra-agent-bootstrap.jar /app/thundra-agent-bootstrap.jar
WORKDIR /app
EXPOSE 8080
ENTRYPOINT [ "java", "-javaagent:thundra-agent-bootstrap.jar", "-jar", "thundra-container-demo.jar" ]
```

### Step 4: Configure the Integration

You need to configure your integration work properly and send your application data to Thundra. You will set the following configurations:

* Thundra API key
* Application Name

You can make these configurations using three different ways:

#### Environment Variables

```
export THUNDRA_APIKEY=<your-api-key> && export THUNDRA_AGENT_APPLICATION_NAME=<your-app-name> && java -javaagent:<path-to-thundra-agent> -jar <your-app-jar> ...
```

#### System Properties

```
java -javaagent:<path-to-thundra-agent> -Dthundra.apiKey=<your-api-key> -Dthundra.agent.application.name=<your-app-name> -jar <your-app-jar> ...
```

#### Configuration File

You need to put `thundra-config.yml` in your artifact, which is picked up from classpath automatically.

```yaml
thundra:
 apiKey: <your-api-key>
 agent:
   application:
     name: <your-app-name>
```

As an example, the `thundra-config.yml` can be put under the `src/main/resources/` folder in the Maven project structure (a software project management tool) so the Thundra agent will be able to directly read its classpath.

![](https://lh4.googleusercontent.com/H2DlIKp8njUWxrrTMVg3gmfDHTOTtrk9fMwLgND75nmDn2ozdTD9xTxjHw\_Nc7FppLP3O6CWoZDUXG09HUMNFcLrjL3FEiR1CUEpBjAKhj8dJ20Id4Io3eLQEGZIpfm1TghsvuWs)

The Thundra agent supports the following Spring frameworks without any additional configuration needed:

* Spring Web (4.x, 5.x)
* Spring Boot (1.x, 2.x)
