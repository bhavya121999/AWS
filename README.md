# Stop and Start Amazon EC2 instances at regular intervals using Lambda

Get the IDs of the EC2 instances that you want to stop and start, and then follow these instructions



 ## Create an IAM policy and role
1.    [Create an IAM policy using the JSON policy editor](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor). Paste this JSON policy document into the policy editor:

![image](https://user-images.githubusercontent.com/46423346/74591960-af758680-5042-11ea-86e1-09ea23095958.png)

2.    [Create an IAM role for Lambda](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console). When attaching a permissions policy, search for and choose the IAM policy that you created.

![image](https://user-images.githubusercontent.com/46423346/74592061-94efdd00-5043-11ea-9f5a-02049ca172ae.png)

## Create Lambda functions that stop and start your EC2 instances
1.    In the [Lambda console](https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions), choose **Create function**.

2.    Choose **Author from scratch.**

3.    Under **Basic information**, add the following:

- For **Function name**, enter a name that identifies it as the function used to stop your EC2 instances. For example, "StopEC2Instances".
- For **Runtime**, choose **Python 3.7**.
- Under **Permissions**, expand Choose or create an execution role.
- Under **Execution role**, choose Use an existing role.
- Under **Existing role**, choose the **IAM role** that you created.

4.    Choose **Create function**.

5.    Copy this code, and then under **Function code**, paste it into the [editor pane in the code editor](https://docs.aws.amazon.com/lambda/latest/dg/code-editor.html#code-editor-code) (lambda_function). This code stops the EC2 instances that you identify.

6.    Under **Basic settings**, set Timeout to 10 seconds.

***Note: Configure the Lambda function settings as needed for your use case. For example, if you want to stop and start multiple instances, you might need a different value for Timeout, as well as Memory.***

7.    Choose Save.

8.    Repeat steps 1-7 to create another function. Do the following differently so that this function starts your EC2 instances:

- In step 3, enter a Function name it as the function used to start your EC2 instances. For example, "StartEC2Instances".
- In step 5, copy and paste this code into the editor pane in the code editor (lambda_function):

***Note: For region and instances, use the same values that you used for the code to stop your EC2 instances.***

![image](https://user-images.githubusercontent.com/46423346/74592081-cbc5f300-5043-11ea-906f-af78117b92a6.png)


## Test your Lambda functions

1.    In the [Lambda console](https://console.aws.amazon.com/lambda/home?region=us-east-1#/functions), choose Functions.

2.    Select one of the **functions** that you created.

3.    Choose **Actions**, and then choose **Test**.

4.    In the **Configure test event** dialog, choose **Create new test event**.

5.    Enter an **Event name**, and then **choose Create**.

***Note: You don't need to change the JSON code for the test event—the function doesn't use it.***

6.    Choose **Test** to execute the function.

7.    Repeat steps 1-6 for the other function that you created.

***Tip: You can check the [status of your EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html#viewing_status) before and after testing to confirm that your functions work as expected.***



## Create rules that trigger your Lambda functions

1.    Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1)

2.    In the **left navigation pane**, under **Events**, choose **Rules**.

3.    Choose **Create rule**.

4.    Under **Event Source**, choose **Schedule**.

5.    Do either of the following:

For **Fixed rate of**, enter an interval of time in minutes, hours, or days.
For **Cron expression**, enter an expression that tells Lambda when to stop your instances. For information on the syntax of expressions, see [Schedule Expressions for Rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html)

***Note: **Cron expressions** are evaluated in UTC. Be sure to adjust the expression for your preferred time zone.***

6.    Under **Targets**, choose **Add target**.

7.    Choose **Lambda function**.

8.    For **Function**, choose the function that stops your EC2 instances.

9.    Choose **Configure** details.

10.    Under **Rule definition**, do the following:

For **Name**, enter a name to identify the rule, such as "StopEC2Instances".
(Optional) For **Description**, describe your rule. For example, "Stops EC2 instances every night at 10 PM."
For **State**, select the Enabled check box.

11.    Choose **Create rule**.

12.    Repeat steps 1-11 to create a rule to start your EC2 instances. Do the following differently:
In step 5, for **Cron expression**, enter an expression that tells Lambda when to start your instances.
In step 8, for **Function**, choose the function that starts your EC2 instances.
In step 10, under **Rule definition**, enter a Name like "StartEC2Instances", and optionally enter a Description like "Starts EC2 instances every morning at 6 AM."

![image](https://user-images.githubusercontent.com/46423346/74592293-d8e3e180-5045-11ea-8f36-fc9d410b5795.png)


