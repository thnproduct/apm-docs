# Integration Options for Node.js SDK in Containers and VMs

#### Step 1: Install the @thundra/core package

```bash
npm install @thundra/core --save
```

#### Step 2: Add Thundra Express middleware &#x20;

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

#### Step 3: Add Thundra API Key

Set the **`thundra_apiKey`** environment variable to the API key value you got from the Thundra console.

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
