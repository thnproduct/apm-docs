# Policies Tab

![Policies Tab](<../../.gitbook/assets/image (214).png>)

In this tab, you can display and manage alert policies that were created for your organization.

![Policies Tab selected](<../../.gitbook/assets/image (122).png>)

Policies are listed in terms of:

* Status - The status of the alert policy. Active alert policies will be listed with their severity in the status bar while inactive alert policies will be listed as OK. Severities could be listed as `INFO`, `WARNING`, or `CRITICAL`.
* Name - Name of the alert policy.
* Targets - The functions or projects for which the alert policy is created. One alert policy can target multiple functions or projects, or even all the functions in the account at the same time. When you hover your mouse over the value listed in this column, you can see the names of these functions or projects.



![Targets Hovered](<../../.gitbook/assets/image (145).png>)

* Notifications - Alert policies can notify more than one channel at the same time. When you hover your mouse over the value in this column, you can see the names of the channels (e.g., Slack channel, URL of the webhook, email address, etc.).

![Notifications Hovered](<../../.gitbook/assets/image (302).png>)

* Enabled - This column lets you quickly disable or enable the alert policy. During times of deployment, you may want to disable certain alerts, and you can do so using the switch in this column.
* Actions - This column contains the action buttons for the alert policy. You can edit the alert policy, delete it, or see its events. When you click on the View Alert Events button, the Events Tab will open and allow you to see the events of the alert policy.

![Alert Policies Checked](<../../.gitbook/assets/image (162).png>)

When you select the checkbox at the beginning of each row, the action buttons for policies will be displayed. You can select one, more than one, or all alert policies to perform bulk operations. Similarly, you can also enable, disable, or delete selected alert policies.
