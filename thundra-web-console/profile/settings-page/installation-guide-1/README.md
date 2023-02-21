# EventBridge Tab

You can integrate Amazon’s EventBridge Service with your Thundra account to receive events from Thunda alerts. See below for the steps you will need to follow in order to send Thundra events to EventBridge.

### Step 1: Complete AWS Integration

To receive events from Thundra on EventBridge, you need to connect Thundra to your AWS account. If you have already integrated Thundra with your AWS account, you can skip this step. For more detailed information about AWS integration, read through our [AWS integration doc](../aws-tab/).

### Step 2: Create Event Source

You need to create an Event Source to send events from Thundra to EventBridge. Navigate to the Settings Page and then click on the EventBridge tab to create an event source. Fill in the following fields and then click the “Save” button:

* **Source Name -** EventBridge source name.
* **AWS ID -** Select your AWS account ID from the drop-down menu. When you integrate your Thundra account with your AWS account, this ID will automatically be displayed. If you cannot see your account ID, make sure you completed the integration described in [Step 1](./#step-1-complete-aws-integration))
* **Region -** Select the AWS region you would like to send the event.

![Create Event Source](<../../../../.gitbook/assets/image (151).png>)

### Step 3: Add EventBridge to Alert Policy

Now you can display your event source when creating or editing an alert policy as a notification target. On the Create/Edit Alert Policy modal, select EventBridge from the Notification drop-down menu. Add the event source that you created on the previous step and click the “Save” button.

![Select EventBridge as Notification Target](<../../../../.gitbook/assets/image (148).png>)

### Step 4: Associate with Event Bus

Using the Amazon EventBridge page in the Partner Event Sources tab, associate the event source with the event bus.

### Step 5: Create Rule

Using the Amazon EventBridge page in the Rules tab, create a rule by selecting the newly created custom event bus.

Thundra events can then be filtered using event patterns (for a detailed Thundra Event payload, see the [Thundra Event Data Model](thundra-event-data-model.md))
