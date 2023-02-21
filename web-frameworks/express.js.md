# Express.js

### Step 1: Install Thundra

```bash
npm install @thundra/core --save
```

### Step 2: Configure Integration

After installing the @thundra/core module, you will need to add Thundra Express middleware to your application. Thundra will monitor your application automatically.

{% code title="app.js" %}
```javascript
const thundra = require("@thundra/core");
const express = require('express');

const app = express()
app.use(thundra.expressMW());

app.get('/', function (req,res) {
   res.send("Response")
});
app.listen(3000)
```
{% endcode %}

### Step 3: Add Thundra API Key

Set the **`thundra_apiKey`** environment variable to the API key value you got from the Thundra console. You can add api key in different ways:

{% code title="app.js" %}
```javascript
const thundra = require("@thundra/core");
// adding apiKey programmatically 
thundra.init({
    apiKey:<Thundra-ApiKey>
})
```
{% endcode %}

{% code title="Shell" %}
```bash
export thundra_apiKey=<Thundra-ApiKey>
```
{% endcode %}

{% code title="Dockerfile" %}
```yaml
ENV thundra_apiKey=<Thundra-ApiKey>
```
{% endcode %}

By default, the name of your app will be `Express` . When you are monitoring multiple express applications you might want to set this to different names for each application, or in general, you might want to describe your applications better. To change the default application name, you can set the `thundra_agent_application_name` environment variable to anything you want.&#x20;
