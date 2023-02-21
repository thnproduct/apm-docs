# FAQ for Python SDK

### How will my function code size be affected?

Thundra tries to affect function size at a minimum level. Since our Python library only has an additional size of 5.1 MB , it will not exaggerate your function code size.

### How can I use Thundra with my Chalice application?

Instrumentation and configuration settings for Chalice are explained [here](https://thundra.gitbook.io/thundra/python/configuration-options/trace-configurations#instrumentation-of-chalice-applications).

### **How can I create custom spans**?

You can use manual instrumentation to create spans for code you would like to monitor. Instructions for performing manual instrumentation for the Python agent can be found [here](https://thundra.gitbook.io/thundra/best-practices/how-to-warmup).

### How can I filter out my functions/invocations with custom labels while searching?

You can add application and invocation tags using our [tagging](https://thundra.gitbook.io/thundra/python/configuration-options/tag-support) feature. On the [Query Bar](https://thundra.gitbook.io/thundra/thundra-web-console/functions-list-page#query-bar) , you can input the tags you added to filter your functions/invocations. For example, let's say that you have functions with different versions and you need to query a specific version. You can add the application tag _**version**_ as the key, and value as a string value, such as _**"1.2.1"**_. The environment variable should be as follows:

```yaml
thundra_agent_lambda_application_tag_version:"1.2.1"
```

Then, using the Query Bar on the [Functions List](../thundra-web-console/functions-list-page/) page, you can search with that filter.

![](<../.gitbook/assets/Screen Shot 2019-09-30 at 16.58.09.png>)

If you want to search through invocations using alabel, you can add invocations tags (see an example [here](https://thundra.gitbook.io/thundra/python/configuration-options/tag-support#adding-invocation-tags).  For our example above, the tag we set should be as follows:

```python
invocation_support.set_tag("version", "1.2.1")
```

Next, search on the Query Bar on the [Function Details](../thundra-web-console/function-details-page/) page using that filter.

![](<../.gitbook/assets/Screen Shot 2019-09-30 at 17.08.54.png>)

### I am getting rate limit errors. How can I fix that?

You can use our sampling feature (described [here](https://thundra.gitbook.io/thundra/python/configuration-options/sampling-support)). By configuring intelligent sampling, you can sample your data without losing interesting data. You can also have a look at this [blog](https://blog.thundra.io/observability-without-breaking-the-bank) to find more information about intelligent sampling.
