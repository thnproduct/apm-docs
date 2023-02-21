# Tagging in Node.js SDK

With Thundra's Node.js agent, you can add custom tags to your invocations or your applications, which will help you better filter the data. Invocation tags are added only to the `Invocation` data type; application tags are global and are automatically added to `Invocation`, `Metric`, `Trace`, `Span`, and `Log` data.‌

## Adding Invocation Tags <a href="#adding-invocation-tags" id="adding-invocation-tags"></a>

You can add custom tags or errors to your invocation by the following methods exported over the main module. The functions that can be used in the module are shown below:

```javascript
static setTag(key: string, value: any): void
static getTag(key: string): any
static removeTag(key: string): void
static setTags(keyValuePairs: {[key: string]: any }): void
static removeTags(): void
static setError(exception: any): void
```

The example below shows how to tag a Lambda invocation(e.g., Lambda Handler):

{% code title="Setting an invocation tag" %}
```javascript
const thundra = require("@thundra/core"); 

exports.handler = thundra()((event, context,callback) => {
    thundra.setTag('userName', 'foo');
    callback(null, 'Hello Thundra!');
});
```
{% endcode %}

## Adding Application Tags <a href="#adding-application-tags" id="adding-application-tags"></a>

You can add application tags via environment variables. All application tags which start with `thundra_agent_lambda_application_tag_` are parsed by Thundra's Node.js agent. The text after the `thundra_agent_lambda_application_tag_` prefix is used as the key of the tag, and the value of the environment variable becomes the value of the tag. For example, `thundra_agent_lambda_application_tag_floatField` with the value `12.123` will add an application tag to all data with the key floatField and the value `12.123`.‌

The example below shows how to add application tags with various data types to a Lambda invocation:

![Setting application tags through the Lambda console](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LjuZ0g3V6GiUFQ\_\_75N%2F-LpTSvqrP-kNAczoXDkw%2F-LpTThfUE7pK54hZ883u%2F3eb238d-application-tag.png?alt=media\&token=38dd45b6-2fa4-429e-9bde-18044c5e5091)
