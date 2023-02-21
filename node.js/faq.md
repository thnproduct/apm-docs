# FAQ for Node.js SDK

### **How will my function code size be affected?**

Thundra tries to affect function size at a minimum level since our Node.JS library only has an additional size of 6.5 MB.

### **How can I create custom spans?**

You can use manual instrumentation to create spans for code that you would like to monitor. Instructions for performing manual instrumentation for the Node.js agent can be found [here](https://thundra.gitbook.io/thundra/node.js/enrich-tracing#using-manual-instrumentation).

### **How can I filter out my functions/invocations with custom labels while searching?**

**You can add application and invocation tags using our** [tagging](https://thundra.gitbook.io/thundra/node.js/nodejs-configuration-options/tag-support). On the [Query Bar](https://thundra.gitbook.io/thundra/thundra-web-console/functions-list-page#query-bar) you can input the tags you added to filter your functions/invocations.

For example, let's say that you have functions with different versions and you need to query a specific version. You can add the application tag _**version**_ as the key, and value as a string value, such as "_**1.2.1**_". The environment variable should be as follow:

```yaml
thundra_agent_lambda_application_tag_version:"1.2.1"
```

Then, on the query bar on **Function List** page you can search with that filter:

![](<../.gitbook/assets/Screen Shot 2019-09-30 at 16.58.09.png>)

If you want to search through invocations using a label, you can add invocation tags (see an example [here](https://thundra.gitbook.io/thundra/node.js/nodejs-configuration-options/tag-support#adding-invocation-tags)). For our example above, the tag we set should be as follows:

```javascript
thundra.InvocationSupport.setTag('version', '1.2.1');
```

Next, search on the Query Bar on the Function Details page using that filter.

![](<../.gitbook/assets/Screen Shot 2019-09-30 at 17.08.54.png>)

### **I am getting rate limit errors. How can I fix that?**

You can use our sampling feature (described [here](https://thundra.gitbook.io/thundra/node.js/nodejs-configuration-options/sampling-support)). By configuring intelligent sampling, you can sample your data without losing interesting data. You can also have a look at this [blog](https://blog.thundra.io/observability-without-breaking-the-bank) to find more information about intelligent sampling.

