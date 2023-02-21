# Serverless Framework

### Adding Thundra libraries with Serverless Framework

If you're using Serverless Framework infrastructure as a code platform to define and deploy your resources, you can easily add Thundra Layer to your project. See the [Node.js](../node.js/nodejs-integration-options.md), [Python](../python/integration-options.md), [Java](../java/integration-options/) and [.NET](../node.js/nodejs-integration-options.md) pages for information about some exceptions according to runtime.

### Instrumenting multiple functions with Thundra's Serverless plugin

Defining Thundra for each function can be a daunting task, which is why we created a Serverless Framework plugin that automatically adds Thundra Layer and instruments all the functions in the .yaml file.

{% hint style="warning" %}
Note that this plugin is currently only available for Java, Python, Node.js and .NET functions.
{% endhint %}

#### Installation

* First, download Thundra’s serverless plugin using the npm command as shown below:

```
npm install serverless-plugin-thundra
```

* After installing Thundra’s serverless plugin, specify it as a plugin for your serverless environment by adding it under the `plugins` section of your .yml file:

```
plugins:
 - serverless-plugin-thundra
```

* Add your Thundra API key by setting the `thundra_apiKey` environment variable:

```
provider:
    environment:
        thundra_apiKey: ${self:custom.thundra.apiKey}
```

{% hint style="warning" %}
Note that some runtime specific configuration should be applied. You may need to check integration options for your function(s) runtime.
{% endhint %}

You can configure Thundra's serverless plugin to disable specific functions or the whole plugin.

#### Disable Plugin:

To disable Thundra's serverless plugin, use the `disable` variable under the Thundra component, which you added under `custom` when adding the `plugin` to your '.yml' file:

```
custom:
  thundra:
    disable: true
```

#### Disable for specific functions:

You may disable Thundra's serverless plugin for some specific function when defining your functions under the `functions` component:

```
functions:
  hello-world-test:
    name: hello-world-test
    handler: index.handler
    custom:
      thundra:
        disable: true
```

Thundra's Serverless Framework plugin allows you to instrument several functions in one go, and also protect your code base from errors that can occur from manual wrapping. However, you still need to configure other aspects of your monitoring requirements. Thundra has several monitoring capabilities, such as instrumentation, to visualize your entire serverless environment in forms of spans. If you would like to enhance your monitoring experience, you will need to perform further configurations, most of which can be performed via environment variables. This prevents you from injecting configurations within your code base, which are prone to errors if not done correctly.
