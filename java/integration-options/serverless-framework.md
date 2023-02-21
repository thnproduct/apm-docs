# Setting up Java SDK with Serverless Framework

{% tabs %}
{% tab title="Serverless Framework" %}
#### **Step 1: Install Thundra’s Serverless Plugin**

```bash
npm install serverless-plugin-thundra
```

#### ****

**Step 2: Add Thundra's Serverless Plugin**&#x20;

After installing Thundra’s Serverless plugin, specify it as a plugin for your serverless environment by adding it under the plugins section of your `serverless.yml` file.

{% code title="serverless.yml" %}
```yaml
plugins:  
    - serverless-plugin-thundra
```
{% endcode %}

#### ****

#### **Step 3: Add Thundra API Key**&#x20;

Add the `thundra` component under `custom` with the `apiKey` set to Thundra API Key under that, as seen below:

{% code title="serverless.yml" %}
```yaml
custom:
    thundra:
        apiKey: <YOUR THUNDRA API KEY>
```
{% endcode %}

#### ****
{% endtab %}
{% endtabs %}
