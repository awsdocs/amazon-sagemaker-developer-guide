# Change an Instance Type<a name="notebooks-run-and-manage-switch-instance-type"></a>

When you open a new notebook for the first time, you are assigned a default Amazon Elastic Compute Cloud \(Amazon EC2\) instance type to run the notebook\. When you open additional notebooks on the same instance type, the notebooks run on the same instance as the first notebook, even if the notebooks use different kernels\.

You can change the instance type that your notebook runs on from within the notebook\.

**Important**  
If you change the instance type, unsaved information and existing settings for the notebook are lost, and installed packages must be re\-installed\.  
The previous instance type continues to run even if no kernel sessions or apps are active\. You must explicitly stop the instance to stop accruing charges\. To stop the instance, see [Shut Down Resources](notebooks-run-and-manage-shut-down.md#notebooks-run-and-manage-shut-down-sessions)\.

The following screenshot shows the menu from a Studio notebook\. The processor and memory of the instance type powering the notebook are displayed as **2 vCPU \+ 4 GiB**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebook-menu-instance.png)

**To change the instance type**

1. Choose the instance type\.

1. In **Select instance**, choose one of the fast launch instance types that are listed\. Or to see all instance types, switch off **Fast launch only**\. The list can be sorted by any column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebook-switch-instance.png)

1. After choosing a type, choose **Save and continue**\.

1. Wait for the new instance to become enabled, and then the new instance type information is displayed\.

For a list of the available instance types, see [Available Instance Types](notebooks-available-instance-types.md)\. 