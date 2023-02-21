# MerLoc GateKeeper Layer Setup

### 1. Layer Setup

#### Layer Setup for Node.js, Python, Java, .NET and Ruby Runtimes

Add MerLoc GateKeeper layer: `arn:aws:lambda:${region}:269863060030:layer:merloc-gatekeeper:${version}`

You can use the latest layer version (shown below) instead of the `${version}` above:

![merloc-gatekeeper](https://api.globadge.com/v1/badgen/aws/lambda/layer/latest-version/us-east-1/269863060030/merloc-gatekeeper)&#x20;

Note that the region of the ARN is dynamic, so you need to change it accordingly to the region where you deploy your function. So let’s say that you deploy your Lambda function to the `Oregon` (`us-west-2`) region. So the layer ARN will be: `arn:aws:lambda:us-west-2:269863060030:layer:merloc-gatekeeper:${version}`

#### Layer Setup for Go Runtime

Add MerLoc GateKeeper Go layer: `arn:aws:lambda:${region}:269863060030:layer:merloc-gatekeeper-go:${version}`

You can use the latest layer version (shown below) instead of the `${version}` above:

![merloc-gatekeeper-go](https://api.globadge.com/v1/badgen/aws/lambda/layer/latest-version/us-east-1/269863060030/merloc-gatekeeper-go) (badge powered by [Globadge serverless](https://www.globadge.com/badges/serverless))

Note that the region of the ARN is dynamic, so you need to change it accordingly to the region where you deploy your function. So let’s say that you deploy your Lambda function to the `Oregon` (`us-west-2`) region. So the layer ARN will be: `arn:aws:lambda:us-west-2:269863060030:layer:merloc-gatekeeper-go:${version}`

*   Configure AWS Lambda Function

    #### Configure for Node.js, Python, Java, .NET and Ruby Runtimes

    Set `AWS_LAMBDA_EXEC_WRAPPER` environment variable to `/opt/extensions/merloc-gatekeeper-ext/bootstrap`

    #### Configure for Go Runtime

    Set runtime to `Custom runtime on Amazon Linux 2` (`provided.al2`)

> **Warning** **`Java 8 on Amazon Linux 1`** (`java8`) and **`.NET Core 3.1`** (`dotnetcore3.1`) runtimes are **not** supported.

* Sign-up Thundra APM [here](https://apm.thundra.io/), get your API key and set your API key through `MERLOC_APIKEY` environment variable.

### 2. Configuration

* `MERLOC_APIKEY`: This configuration is **OPTIONAL**. But it is required (**MANDATORY**) if the broker requires API key (for example Thundra hosted MerLoc broker). If you are using Thundra hosted MerLoc broker (which is default), you can sign-up Thundra APM [here](https://apm.thundra.io/) and get your API key.
*   `MERLOC_ENABLE`: This configuration is **OPTIONAL**. Even though MerLoc GateKeeper layer is added and configured, you can disable it by setting the `MERLOC_ENABLE` environment variable to `false`. For example,

    ```
    MERLOC_ENABLE=false
    ```
*   `MERLOC_DEBUG_ENABLE`: This configuration is **OPTIONAL**. By default, internal debug logs are disabled, but you can enable it by setting the `MERLOC_DEBUG_ENABLE` environment variable to `true`. For example,

    ```
    MERLOC_DEBUG_ENABLE=true
    ```
*   `MERLOC_BROKER_CONNECTION_NAME`: This configuration is **OPTIONAL**. By default, the name of the connection to the broker is the name of the AWS Lambda function for GateKeeper. But you can change it by setting the `MERLOC_BROKER_CONNECTION_NAME` environment variable. For example,

    ```
    MERLOC_BROKER_CONNECTION_NAME=serkan-connection
    ```
