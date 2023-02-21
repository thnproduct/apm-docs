# Integration Options for Java SDK in Containers and VMs

## Download the Agent

Download the latest Thundra agent from the Thundra [repo](https://repo.thundra.io/service/local/artifact/maven/redirect?r=thundra-releases\&g=io.thundra.agent\&a=thundra-agent-bootstrap\&v=LATEST).&#x20;

## Configure the Agent

In order to configure the agent, you'll need an API key from Thundra APM.&#x20;

### Configure by Environment Variable

The Thundra agent can be configured externally through environment variables:

* Configure the Thundra API key by setting the `THUNDRA_APIKEY` environment variable.
* Configure application name by setting the `THUNDRA_AGENT_APPLICATION_NAME` environment variable.

So, a sample configuration will look like this:

```
export THUNDRA_APIKEY=<YOUR-THUNDRA-API-KEY> 
export THUNDRA_AGENT_APPLICATION_NAME=user-service
```

### Configure by YAML format

Thundra agent can be configured through a YAML formatted configuration file named `thundra-config.yml` in the classpath (for example the config file might be located under `src/main/resources` directory):

* Configure the Thundra API key by setting the `thundra.apiKey` YAML key.&#x20;
* Configure the application name by setting the`thundra.agent.application.name` YAML key.&#x20;

So, a sample configuration file `thundra-config.yml` will look like this:

```yaml
thundra:
 apiKey: <YOUR-THUNDRA-API-KEY>
 agent:
   application:
     name: user-service

```

### Sample Docker Configuration

Here you can see a sample Docker configuration. It may differ from your own Docker configuration. **You need to modify this sample according to your own Docker configuration.**

```bash
FROM openjdk:8
RUN mkdir -p /app

# Copy app artifact
ADD target/<Your-App>.jar /app/<Your-App>.jar

# Copy Thundra bootstrap agent artifact. 
# Please rename with the file you have downloaded at the first step.
ADD thundra-agent-bootstrap.jar /app/thundra-agent-bootstrap.jar

WORKDIR /app
EXPOSE 8080
ENTRYPOINT [ "java", "-javaagent:thundra-agent-bootstrap.jar", "-jar", "<Your-App>.jar" ]
```

You can use the example below if you would like to use environment variables with `docker run`

```
docker run ... \ 
    -e THUNDRA_APIKEY=<YOUR-THUNDRA-API-KEY> \
    -e THUNDRA_AGENT_APPLICATION_NAME=<YOUR-APP-NAME> \
    ...
```

## Add Thundra to Your Application

Run the following command to add Thundra to your application.&#x20;

```bash
java -javaagent:<path-to-thundra-agent> -jar <your-app-jar> ...
```
