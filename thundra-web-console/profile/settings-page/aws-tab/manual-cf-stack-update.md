# Manual CF Stack Update

As you may know, you can use Thundra easily using Thundra AWS integration. We are using Thundra CloudFormation stack to monitor your AWS services with a single step. However, sometimes we're updating our CF stack to extend our features. You need to make sure that your stack is updated to take advantage of these features!

The update process is simpler for customers who already have a recent version of Thundra’s CloudFormation stack. They can easily update their stack with one click on the [AWS Settings page](https://console.thundra.io/settings/aws). However, you may need to make this update manually just for one time. \
\
You can follow the steps below to manually update your Thundra CloudFormation stack to the latest version.

**Step 1:** In the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation), from the list of stacks, select the Thundra CloudFormation stack.&#x20;

{% hint style="info" %}
The default name we provide for the stack is **ThundraCloudformationStack.** It should be on the list if you didn't change it during account integration.
{% endhint %}

**Step 2:** In the stack details pane, choose **Update**.

**Step 3:** Select **Replace current template** and set S3 URL to the value below and click **Next**:

```
http://thundra-dist.s3.amazonaws.com/thundra-template.yml
```

![](<../../../../.gitbook/assets/image (93).png>)

**Step 4:** On the next step parameters will show up as seen below. No modification is required on these values, you can directly click to **Next**

![](<../../../../.gitbook/assets/image (81).png>)

**Step 5:** On the next step it is possible to update stack options. No modification is required on this step, you can directly click to **Next.**

![](<../../../../.gitbook/assets/image (33).png>)

**Step 6:** On the final step, a review of the update operation is shown. You need to scroll down to the bottom of the page and check the confirmation box as shown below and click to **Update Stack.**

![](<../../../../.gitbook/assets/image (56).png>)

**Step 7:** Wait for the stack update to finish. This process can take up to one minute.
