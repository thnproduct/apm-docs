# ServiceNow Tab



In Thundra, you can create an alert policy that will help you stay aware of issues in your serverless architecture. When setting alerts, you can select different ways to receive notifications, including having them sent to your ServiceNow app as incidents. To have your notifications sent to ServiceNow, you need to set up your settings in the ServiceNow tab.

Before you can integrate your ServiceNow application to Thundra, you need to create a user on ServiceNow to use for webhook authentication.

To begin this process, first, navigate to Organization --> Users using the search bar on your ServiceNow instance.

![Navigate and Create User](<../../../../.gitbook/assets/image (156).png>)

Click on “New” to create a new user for this integration. On the form that pops up, fill in the following areas:

* User ID
* First Name
* Check Active
* Check `Web Service Access Only`
* Check `Internal Integration User`



![Create User](<../../../../.gitbook/assets/image (266).png>)

When you click the “Submit” button, the new user will be created. Of note, this user should be assigned specific roles to enable integration. Click on the “Edit” button on the Roles tab, and add the `itil` and `rest_service` roles. Once this is done, the user is ready and you can begin integrating with Thundra.

Making a ServiceNow configuration in Thundra is very easy and straightforward. Navigate to the Settings page and click on the ServiceNow tab. Enter the instance id of your ServiceNow instance (the instance id should include only the subdomain). For example, the instance id for this `https://dev12345.service-now.com/` should be `dev12345`. Enter the user name and password that you used for the ServiceNow user that you created for this integration. Finally, save the configuration settings. Now you can display your ServiceNow instance when [creating an alert](../../../alerts-page/creating-editing-alert-policies.md).

If you want to remove your integration, just click on the “Remove” button. However, if you have alert policies set up with a ServiceNow notification, you cannot delete this instance.&#x20;

To navigate to the ServiceNow tab, just click on the User icon and the Settings item and go to Alert Integrations tab. Then you will be able to open the ServiceNow tab.
