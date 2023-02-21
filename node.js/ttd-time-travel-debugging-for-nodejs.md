# TTD (Time Travel Debugging) for NodeJS

TTD (TimeTravel Debugging) makes it possible to travel back in time to previous states of your application by getting a snapshot of when each line is executed. You can step over each line of the code and track the values of the variables captured during execution.&#x20;

With the help of TTD (TimeTravel Debugging), you can see how your applications behave at every level, giving you in-depth monitoring capabilities along with micro-tracing capability even inside the method by tracing code execution line by line with the snapshots (values) of arguments, local variables and fields.

TTD (TimeTravel Debugging) is disabled by default,  but can be enabled by **configuration** (environment variables, etc ...).

![Time Travel Debugging (TTD) Panel](<../.gitbook/assets/TTD - Node.js.png>)

## **Configuration**

To configure TTD (Time Travel Debugging) behavior through environment variables **without any code change**, the `traceLineByLine` attribute of the `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG` environment variable must be configured for the modules/files or methods to be traced line by line.

Environment variable names must be in the `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG...` format, meaning a name must start with the `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG`. For example, `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG`, `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG1`, `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG2`, etc ... So multiple definitions can be defined through multiple environment variables starting with `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG` environment variable name.

Environment variable values must be in the `<module-def>.<method-def>[...]` format, The asterisk character (`*`) in the `<module-def>` and `<method-def>` is supported. For example

* If you want to enable TTD (Time Travel Debugging) for all methods in a module (let say that `src/user/service.js)` using environment variables, the `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG` environment variable can be specified as `src.user.service.*[traceLineByLine=true].`

```yaml
THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG: src.user.service.*[traceLineByLine=true]
```

Here, `src.user.service` part specifies the file location of your module, and `*` means line by line tracing for **all** the methods in this file will be enabled.&#x20;

* If you want to debug just one method (for ex. `getUser`), in a module (let say that `src/user/service.js`) you can specify its name directly, such as:

```yaml
THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG: src.user.service.getUser[traceLineByLine=true]
```

Here, `src.user.service` part specifies the file location of your module, and `getUser` means line by line tracing will be enabled only for the `getUser` method in this file.&#x20;

* If you need to enable TTD (Time Travel Debugging) for all files under a directory (let say that `src/user/)`, you can specify the directory name and owned files by using wildcard, such as:

```yaml
THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG: src.user.*.*[traceLineByLine=true]
```

Here `src.user.*` part specifies **all** the files located under `src.user` directory, and `*` means line by line tracing for **all** the methods in these files will be enabled.&#x20;

* If you need to enable TTD (Time Travel Debugging) for multiple files in different folders/files, you can specify multiple configurations using the same environment variable key, `THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG`, as the prefix, such as in the following example:

```yaml
THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG_1: src.user.service.*[traceLineByLine=true]
THUNDRA_AGENT_TRACE_INSTRUMENT_TRACEABLECONFIG_2: src.product.api.*[traceLineByLine=true]
```

## **Using With Webpack**

If you are using **Webpack** to bundle your applications, you need use [thundra-webpack-plugin](https://github.com/thundra-io/thundra-webpack-plugin#readme) to activate TTD (Time-Travel Debugging) during compilation.

### Install Webpack Plugin

```bash
npm install --save-dev @thundra/webpack-plugin
```

### Configure Webpack Plugin

In your Webpack config file (lets say `webpack.config.js`), you need to configure and add Thundra Webpack plugin as shown in the following example:

{% code title="webpack.config.js" %}
```javascript
const { ThundraWebpackPlugin } = require('@thundra/webpack-plugin');

module.exports = {
    mode: 'development',
    target: 'node',
    plugins: [new ThundraWebpackPlugin([
        'src.user.service.*[traceLineByLine=true]',
    ])],
}
```
{% endcode %}

To use the plugin, an instance of the `ThundraWebpackPlugin` should be added to the list of plugins in the Webpack configuration file. The constructor for `ThundraWebpackPlugin` requires a list of strings, where each string defines the file and the methods that you want to instrument in the syntax/format as described in the **Configuration** section above.

## **Using With Esbuild**

If you are using **Esbuild** to bundle your applications, you need use [thundra-**esbuild**-plugin](https://github.com/thundra-io/thundra-esbuild-plugin/blob/master/README.md) to activate TTD (Time-Travel Debugging) during compilation.

### Install Esbuild Plugin

```
npm install --save-dev @thundra/esbuild-plugin
```

### Configure Esbuild Plugin

In your build script file (where you initiate and configure **Esbuild**), you need to configure and add Thundra Esbuild plugin as shown in the following example:

{% code title="build.js" %}
```javascript
const { ThundraEsbuildPlugin } = require('@thundra/esbuild-plugin');

require('esbuild').build({
  entryPoints: ['...'],
  bundle: ....,
  outfile: ...,
  plugins: [
    ThundraEsbuildPlugin({
      traceableConfigs: [
        'src.api.*.*[traceLineByLine=true]', // activate line by line tracing for all files/methods under src/api folder
      ]
    })
  ],
});
```
{% endcode %}

To use the plugin, `ThundraEsbuildPlugin` instance must be added into plugins. The constructor of `ThundraEsbuildPlugin` takes the following arguments to be configured:

| Name             | Requirement | Description                          |
| ---------------- | ----------- | ------------------------------------ |
| traceableConfigs | Required    | Array of instrumentation definitions |

Syntax/Format of the `traceableConfigs` argument is described in the **Configuration** section above.

### **Using With SST**

If you use SST and **your code is bundled**, you can use [thundra-**esbuild**-plugin](https://github.com/thundra-io/thundra-esbuild-plugin/blob/master/README.md) to activate TTD (Time-Travel Debugging).

You can create an **Esbuild** configuration file as shown in the following example:

{% code title="esbuild.js" %}
```javascript
const { ThundraEsbuildPlugin } = require('@thundra/esbuild-plugin');

module.exports = {
  ...
  plugins: [
    ThundraEsbuildPlugin({
      traceableConfigs: [
        'src.api.*.*[traceLineByLine=true]', // activate line by line tracing for all files/methods under src/api folder
        // see Configure Esbuild Plugin title for expression details
      ]
    })
  ]
};
```
{% endcode %}

And then, in resource definition (like `Function`) at your stack, add the **Esbuild** plugin in the `bundle` configuration as show in the following example:

{% code title="stacks/MyStack.js" %}
```javascript
new Function(this, "MyFunction", {
    // ...
    handler: "src/index.main",
    bundle: {
        esbuildConfig: {
            plugins: "esbuild.js",
        },
    },
    // ...
});    

```
{% endcode %}
