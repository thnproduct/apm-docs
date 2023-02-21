# Architectural View to Check The Issues at a Glance

When you go into serverless world to build an application, most probably you will start to build your logic within Lambda functions. Your functions could interact with other AWS services such as writing data to Dynamo DB or triggering from an API Gateway. Day by day number of functions  will be grown to optimize your system or make readable your code. In the end, you will have an complex system that has many functions and various service interactions. When you have a complex system like mentioned,  it is hard to detect bottlenecks and track problems by looking in functions. At this point, Thundra provides you to see your overall architecture and detect errors or performance issues in a single view: Architectural View.

![](<../.gitbook/assets/image (102).png>)

In [Architecture](../thundra-web-console/architecture-page.md) page, architecture of your system is presented to you with color coded edges and grouped by projects. Grouping your relevant functions under one project helps you to view more meaningful architecture maps.

Thundra's architecture view offers you to detect problems in your architecture at a glance. Colors of edges between resources represent different health states of interaction. For example, if edge is green, you could be relax, that means the interaction is healthy. However if you see yellow, orange or red edges between your Lambda and resources,  it indicates there is something that retrogrades. In favor of this visual representation of your architecture, you can easily focus on problematic areas.&#x20;

In addition to the color-coded visualizations of the edges, they also highlight the number of interaction between the resources by its thickness.

Do you notice that some nodes have a bigger size than the other ones? Architecture view helps you to differentiate the throughput on the resources. Resources with the bigger size have more throughput than the smaller ones.&#x20;

### Diagnosis

You displayed problematic interaction but you may want to take a closer look to know why this error occur? When you click on an edge, details of specific interaction is displayed at the right side of the [Architecture Page](../thundra-web-console/architecture-page.md). In this part, you can see a [summary](../thundra-web-console/architecture-page.md#summary) of how the interaction between functions is performing.&#x20;

For more details, you are able to see time series charts that visualize how number of healthy/erroneous interactions and durations change between your function and resource.&#x20;

You can also display specific invocations of the function that are contained in selected interaction. When you click on an invocation you will be redirected to [Invocation Detail Page](../thundra-web-console/invocation-detail-page/) to have a deeper insight about specific invocation.

![](<../.gitbook/assets/image (246).png>)

### Track Changes&#x20;

We all know that during building an application, we make a lots of changes to improve performance of system or add new component to application. Thundra's [Architecture Page](../thundra-web-console/architecture-page.md) allows you to see what changes within in a timeline. Using time slider at the bottom of the page, you can track changes and see effects of these changes in your architecture.

![Time Slider](<../.gitbook/assets/image (121).png>)

### Customize Your Architecture View

Architecture View displays resources, interactions between your Lambda functions and other resources and health of these interactions. You can customize these visualization using Filters button in architecture view. This allows you to display more detailed information on your architecture view.

You can display health of interactions by colors of edges. If you enable "Show Metrics" in Filters, you will be able to see how many times your function interacted with the resource and the average duration with that function took in execution with the resource.

You can show or hide labels of resources using "Show Labels" option in Filters.

If you have trouble to get used to new icons of AWS you can change the icons of resources by selecting "Old AWS icons".



