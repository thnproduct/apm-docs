# Changing local root

While debugging a lambda function using Thundra's Online Debugger, if the local root does not match the remote root; VSCode might not be able to resolve the breakpoint locations. If you put a breakpoint in your Lambda function, the function is connected however debugger is not stopping at the breakpoint; the problem might be related to the local root.

To solve this issue you can use the **"Thundra: Change local root"** command provided by the Thundra's VSCode extension.

In the below example we have a directory opened in VSCode, containing two separate Lambda functions.

![](<../../.gitbook/assets/image (2).png>)

When you deploy these two lambda functions and try to debug the first Lambda function using Thundra's online debugger, you will see the function will not stop at the breakpoint.

That's because VSCode couldn't resolve the incoming breakpoint location in the current workspace. To resolve this issue, you can use **"Thundra: Change local root"** command.

![](<../../.gitbook/assets/image (80).png>)

Since we want to debug the first lambda function we can set the local root for the debugger to **"./firstLambda/"** directory.

![](<../../.gitbook/assets/image (86).png>)

After setting the local root, when you try to start the debugger again and invoke the function; you will see that the function will stop at the breakpoint that we have put.

![](<../../.gitbook/assets/image (38).png>)
