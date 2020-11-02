# Ground Truth Security and Permissions<a name="sms-security-general"></a>

Use the topics on this page to learn about Ground Truth security features, and how to configure AWS Identity and Access Management \(IAM\) permissions to allow an IAM user or role create a labeling job\. Additionally, learn how to create an *execution role*\. An execution role is the role that you specify when you create a labeling job and it is used to execute your labeling job\.

If you are a new user and want to get started quickly, or if you do not require granular permissions, see [Grant General Permissions To Get Started Using Ground Truth](sms-security-permission.md#sms-security-permissions-get-started)\.

**Important**  
When you create your labeling job, if you set the Task time limit \(`TaskTimeLimitInSeconds` when using the Amazon SageMaker API\) to be greater than one hour \(3,600 seconds\), you must increases the *max session duration* of your execution role to be greater than or equal to the task timeout\.  
You can modify the max session duration of your execution rule using the IAM console, AWS CLI, and IAM API\. To modify your execution role, go to [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, select your preferred method \(console, CLI, or API\) to modify the role from the **Topics** list, and then select **Modifying a Role Maximum Session Duration** to view the instructions\. For 3D point cloud task types, refer to [Increase MaxSessionDuration for Execution Role](sms-point-cloud-general-information.md#sms-3d-pointcloud-maxsessduration)\.

For more information about IAM users and roles, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the IAM User Guide\. 

To learn more about using IAM with SageMaker, see [Identity and Access Management for Amazon SageMaker](security-iam.md)\.

**Topics**
+ [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)
+ [Data and Storage Volume Encryption](sms-security.md)
+ [Workforce Authentication and Restrictions](sms-security-workforce-authentication.md)