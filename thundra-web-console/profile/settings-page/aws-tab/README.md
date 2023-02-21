# AWS Tab

Integrating Thundra with AWS enables you to get metrics and logs data from CloudWatch. As a result of AWS integration, Thundra can retrieve logs from CloudWatch synchronously and no overhead will be added over the duration. Thundra can also find your AppSync APIs through CloudWatch integration.

The AWS integration page can be reached by going to the Settings page and then clicking on the AWS tab.

![](<../../../../.gitbook/assets/image (18).png>)

To gather your CloudWatch logs, Lambda information, and API data, you will need to deploy a CloudFormation stack that belongs to Thundra. The CloudFormation stack requires necessary access for monitoring, and can be deleted at any time through the AWS CloudFormation page.

Follow the steps below to create a successful integration between AWS and Thundra.

**Step 1:** Click the “Add New Account” button and then, on the window that opens, click on “Connect Thundra” to deploy the Thundra CloudFormation stack.

**Step 2:** A CloudFormation creation wizard will open in a new tab.

**Step 3:** Check the “I Acknowledge that AWS CloudFormation might create IAM resources” box, and click “Create.”

**Step 4:** Wait for the stack creation to finish. This process can take up to one minute.

![](<../../../../.gitbook/assets/image (91).png>)

**Step 5:** Select functions that you want to subscribe on the list that pops up.

**Step 6:** Check the box at this step if you want to monitor your AppSync APIs.

After you finish, you will see the functions that Thundra subscribed to on your AWS account in the [Functions List](../../../functions-list-page/) page and APIs on your account on the [APIs](../../../apis-page.md) page.

As your functions are invoked, the logs, metrics, and base information of the invocation (such as duration and errors) will be available in Thundra. When requests are arrived to your APIs, it will be available on the APIs page.

### AWS Account Details

When you click on an account, you can display its details. At the top of the tab, you can see the main information of your account. Some of your abilities in this section include:

![](<../../../../.gitbook/assets/image (99).png>)

* Changing the alias of your account so you can more easily recognize it when you look at the AWS account list.
* Sync your AWS account to get latest changes such as new functions or APIs.
* You can remove the connection with your AWS account by clicking "Delete" button. You will be redirected to Thundra AWS CloudFormation stack and you need to delete this stack.

#### Updating CloudFormation Stack

If you are using an older version of CloudFormation Stack, you will see a warning to update it. You need to update your stack to keep in track your Thundra experience. When you click on the update link, you can follow the instructions for updating process.

#### Functions Tab

You can list and manage your functions under connected AWS account page.

* Selecting “Autosubscribe,” which defines whether or not Thundra will automatically subscribe to a new function you add to your AWS account.

**Unsubscribed/Subscribed Functions Lists**

![](<../../../../.gitbook/assets/image (108).png>)

These lists give you the ability to display and manage unsubscribed or subscribed functions. Select a function from these lists and then click on either the “Subscribe” button on the “Unsubscribed Functions List” or the “Unsubscribe” button on the Subscribed Functions List.

* If there is an error during the subscription or unsubscription process, a warning icon will pop up. Hover your mouse over this icon to display the cause of the error.
* You can display the number of functions that end with an error using the badges on the top right side of the lists. Errors mostly occur if a function you tried to subscribe to Thundra is already subscribed to another resource. Go to the FAQ section for information about how to fix errors.
* Refresh the lists after you subscribe or unsubscribe functions to see the results of your changes.

#### APIs Tab

You can list and manage your APIs under connected AWS account page.

* Selecting “Autosubscribe,” which defines whether or not Thundra will automatically subscribe to a new API you add to your AWS account.

**Unsubscribed/Subscribed APIs**

![](<../../../../.gitbook/assets/image (61).png>)



These lists give you the ability to display and manage unsubscribed or subscribed APIs. Select an API from these lists and then click on either the “Subscribe” button on the “Unsubscribed API List” or the “Unsubscribe” button on the Subscribed API List.

* If there is an error during the subscription or unsubscription process, a warning icon will pop up. Hover your mouse over this icon to display the cause of the error.
* You can display the number of APIs that end with an error using the badges on the top right side of the lists. Errors mostly occur if a function you tried to subscribe to Thundra is already subscribed to another resource. Go to the FAQ section for information about how to fix errors.
* Refresh the lists after you subscribe or unsubscribe APIs to see the results of your changes.
