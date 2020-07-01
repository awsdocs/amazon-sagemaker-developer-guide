# Step 1: Setting up your workforce<a name="sms-custom-templates-step1"></a>

In this step you use the console to establish which worker type to use and make the necessary sub\-selections for the worker type\. It assumes you have already completed the steps up to this point in the [Getting started](sms-getting-started.md) section and have chosen the **Custom labeling task** as the **Task type**\.

**To configure your workforce\.**

1. First choose an option from the **Worker types**\. There are three types currently available:
   + **Public** uses an on\-demand workforce of independent contractors, powered by Amazon Mechanical Turk\. They are paid on a per\-task basis\.
   + **Private** uses your employees or contractors for handling data that needs to stay within your organization\.
   + **Vendor** uses third party vendors that specialize in providing data labeling services, available via the AWS Marketplace\.

1. If you choose the **Public** option, you are asked to set the **number of workers per dataset object**\. Having more than one worker perform the same task on the same object can help increase the accuracy of your results\. The default is three\. You can raise or lower that depending on the accuracy you need\.

   You are also asked to set a **price per task** by using a drop\-down menu\. The menu recommends price points based on how long it will take to complete the task\.

   The recommended method to determine this is to first run a short test of your task with a **private** workforce\. The test provides a realistic estimate of how long the task takes to complete\. You can then select the range your estimate falls within on the **Price per task** menu\. If your average time is more than 5 minutes, consider breaking your task into smaller units\.

## Next<a name="templates-step1-next"></a>

[Step 2: Creating your custom labeling task template](sms-custom-templates-step2.md)