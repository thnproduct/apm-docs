# Setting up Java SDK as Lambda Container Image

{% tabs %}
{% tab title="Docker Image Integration" %}
#### **Step 1: Pull the Thundra base image via Docker CLI**&#x20;

```
docker pull thundraio/thundra-lambda-container-base-java8:LATEST
```

#### ****

#### **Step 2: Configure Your Dockerfile**

* Add Thundra base image as parent

```
FROM thundraio/thundra-lambda-container-base-java8:LATEST
```

* (**Optional**) Add following line to your Dockerfile **if you want use a different version** of the Thundra agent instead of the one coming with the base image.

```
RUN /opt/thundra/thundra-version-updater.sh <version>
```

The latest available version you can use to replace the `<version>` parameter

![](https://img.shields.io/nexus/thundra-releases/io.thundra.agent/thundra-agent-lambda-core?server=http%3A%2F%2Frepo.thundra.io)

* Make sure your function code is added to the task directory of your docker image and handler is set as the first argument of CMD

```
# your function code path
ARG FUNCTION_CODE_PATH="target/sample-app.jar"

# task directory
ARG TASK_DIR="/var/task"

# create task directory
RUN mkdir -p ${TASK_DIR}

# copy your function code to the task directory
COPY ${FUNCTION_CODE_PATH} ${TASK_DIR}

# Set the CMD to your handler
CMD ["io.thundra.container.sample.Handler"]

```

{% hint style="info" %}
You can set your handler also by parameter overriding outside of the Dockerfile or setting it as the value of Lamba environment variable `THUNDRA_AGENT_LAMBDA_HANDLER`
{% endhint %}

An example DockerFile:

```
FROM thundraio/thundra-lambda-container-base-java8:LATEST

RUN /opt/thundra/thundra-version-updater.sh 2.6.16

# your function code path
ARG FUNCTION_CODE_PATH="target/sample-app.jar"

# task directory
ARG TASK_DIR="/var/task"

# create task directory
RUN mkdir -p ${TASK_DIR}

# copy your function code to the task directory
COPY ${FUNCTION_CODE_PATH} ${TASK_DIR}

# Set the CMD to your handler
CMD ["io.thundra.container.sample.Handler"]
```

****

**Step 3: Build your container image**

```
docker build -t <image-name>
```

****

**Step 4: Locally test your  container image**

* Run your Lambda application locally

```
docker run -p 9000:8080 <image name>
```

* Invoke your local application

```
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}'Step 3: Invoke Your Deployed Function
```

You should see your function’s expected response as the output of this request. You can also see the invocation logs on the console you run the Docker image.\


**Step 4: Push the container image to ECR and deploy it to your Lambda function**

****

**Step 5: Configure your function**

Set the **`THUNDRA_APIKEY`** environment variable to the API key value you got from the Thundra Console.

&#x20;
{% endtab %}
{% endtabs %}

