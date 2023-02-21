# MerLoc CLI Setup & Usage

**MerLoc CLI** is a client side CLI tool to run AWS Lambda functions on your local. It communicates with **MerLoc GateKeeper** in AWS Lambda function over **MerLoc Broker** for receiving invocations, executing function locally and then returning response to AWS Lambda service.

### 1. Prerequisites

* Node.js 14+
* Docker (**installed and running**)



### 2. Setup

```
npm install -g merloc-cli
```

After install, check whether it is installed successfully:

```
merloc --version
```

By this command, you should see the installed version number if everything is installed properly.

### 3.  How to Run

**MerLoc CLI** (`merloc` command) must be run in your serverless project root directory, so it can use the integrated tool (`AWS SAM`, `Serverless Framework`, ...) to run function locally.

### 4. Configuration

*   `-c <name>` (or `--connection-name <name>`): Configures name of the connection to the **MerLoc Broker** This configuration is **OPTIONAL**. The default value is `default`. Connection with name `default` matches with all **MerLoc GateKeeper** connections if there is no another local connection with the function name (or connection name set by `MERLOC_BROKER_CONNECTION_NAME` at **MerLoc GateKeeper**) specifically. For example:

    ```
    merloc -c my-connection
    ```
*   `-a <key>` (or `--api-key <key>`): Configures API key for authorization while connecting to **MerLoc Broker**. For example:

    ```
    merloc -a 1234-5678-90
    ```
*   `-i <name>` (or `--invoker <name>`): This configuration is **OPTIONAL** (but it is highly recommended to be set according to integrated tool). The default value is `auto`. Valid values are:

    * `auto`: Automatically decides invoker to be used (`sam-local` or `serverless-local`). Decision is taken based on project structure:
      * If there is `template.yml` in the project root directory, `sam-local` invoker is used.
      * If there is `serverless.yml` in the project root directory, `serverless-local` invoker is used.
      * Otherwise, terminates **MerLoc CLI** with error as invoker to be used couldn't be determined.
    * `sam-local`: Uses **AWS SAM** local to run and debug function locally. To use this option, the project must be a valid **AWS SAM** project.
    * `serverless-local`: Uses **Serverless** local to run and debug function locally. To use this option, the project must be a valid **Serverless framework** project. For example:

    ```
    merloc -i sam-local
    ```
*   `-d` (or `--debug`): Enables breakpoint debugging (if supported for the function runtime by **MerLoc**). By default breakpoint debugging is disabled. For example:

    ```
    merloc -d
    ```

{% embed url="https://www.youtube.com/watch?v=mXNnT5aTVC4&ab_channel=Thundra" %}

*   `-r` (or `--reload`): Enables hot-reloading (if supported for the function runtime by **MerLoc**). This configuration is **OPTIONAL**. By default, hot-reloading is disabled. For example:

    ```
    merloc -r
    ```

    **Note:** Hot-reload can also be triggered manually by pressing `Ctrl+R`.

{% embed url="https://www.youtube.com/watch?v=O5tYKVpncdo&ab_channel=Thundra" %}

*   `-w <paths...>` (or `--watch <paths...>`): Configures paths to files, directories or **glob** patterns to be watched for changes to trigger hot-reload automatically. Enabling hot-reload (by `-r` or `--reload` as mentioned above) is required to use this option. This configuration is **OPTIONAL**. By default, current directory is watched. For example:

    ```
    merloc -r -w '**/*.ts' '**/*.js'
    ```

    **Note:** The following file patterns are ignored from being watched:

    * `'**/.idea/**'`
    * `**/.vscode/**'`
    * `**/.github/**'`
    * `**/.serverless/**'`
    * `**/.aws-sam/**'`
    * `**/.build/**'`
    * `**/.*'`
    * `**/*.json'`
    * `**/*.yml'`
    * `**/*.md'`
    * `**/*.txt'`
    * `**/LICENSE'`
*   `-rc <mode>` (or `--runtime-concurrency <mode>`): Configures concurrency level at runtime level globally. This configuration is **OPTIONAL**. The default value is `reject`. Valid values are:

    * `reject`: Rejects any invocation from any function if local runtime is busy executing other invocation. In this case, **MerLoc GateKeeper** is informed by local runtime with the rejection and then GateKeeper forwards the request to the original user handler.
    * `wait`: Waits any invocation from any function until runtime is available if local runtime is busy executing another invocation.
    * `per-function`: Disables global lock between functions and runtime lock is handled at per function basis according to `-fc <mode>` (or `--function-concurrency <mode>`) configuration mentioned below. For example,

    ```
    merloc -rc per-function
    ```
*   `-fc <mode>` (or `--function-concurrency <mode>`): Configures concurrency level at function level so executing a function doesn't block executing another function. This configuration is **OPTIONAL**. The default value is `reject`. Valid values are:

    * `reject`: Rejects an invocation from a function if local runtime is busy executing other invocation of that function. In this case, **MerLoc GateKeeper** is informed by local runtime with the rejection and then GateKeeper forwards the request to the original user handler.
    * `wait`: Waits an invocation from a function until runtime is available if local runtime is busy executing another invocation of that function.

    For example,

    ```
    merloc -fc wait
    ```
*   `-v` (or `--verbose`): Enables verbose mode to print internal logs of the **MerLoc CLI**. For example:

    ```
    merloc -v
    ```
*   `-version`: Prints version of the currently installed **MerLoc CLI**. For example:

    ```
    merloc --version
    ```
*   `-h` (`--help`): Displays help for **MerLoc CLI**. For example:

    ```
    merloc -h
    ```

### 5. Integrations

#### 5.a. AWS SAM

**MerLoc CLI** (`merloc` command) must be run in your **AWS SAM** project root directory where `template.yml` file is located.

Go to AWS SAM page for the details.

#### 5.b. Serverless Framework

**MerLoc CLI** (`merloc` command) must be run in your **Serverless Framework** project root directory where `serverless.yml` file is located.

Serverless Framework page for the details.

### 6. Troubleshooting

*   If you see the following error message

    ```
    ERROR - <index> Unable to connect to broker: Error: Unexpected server response: 403
    ```

    while connecting broker, this means that,

    * either your broker requires an API key (for ex, you are using Thundra hosted broker) and you didn't provide an API key by `-a <api-key>` option
    * or the API key you provided is invalid
