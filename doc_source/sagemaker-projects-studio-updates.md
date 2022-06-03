# SageMaker Studio Permissions Required to Use Projects<a name="sagemaker-projects-studio-updates"></a>

Users can view SageMaker provided project templates and create projects with those templates when you grant **Projects** permissions for users\. You can grant these permissions when you onboard or update Amazon SageMaker Studio\. There are two permissions to grant\.

1. Grant **Projects** permissions for the Studio administrator to permit the Studio administrator to view the SageMaker\-provided templates in the AWS Service Catalog console\. The administrator can see what other Studio users create if you grant them permission to use SageMaker projects\. The administrator can also view the AWS CloudFormation template that the SageMaker\-provided project templates define in the AWS Service Catalog console\. For information about using the AWS Service Catalog console, see [What Is AWS Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) in the *AWS Service Catalog User Guide*\.

1. Allow Studio users who are configured to use the domain execution role to create projects\. This grants Studio users permission to use the SageMaker\-provided project templates to create a project from within Studio\.

For users who use any role other than the domain execution role to view and use SageMaker\-provided project templates, you need to grant **Projects** permissions to the individual user profiles\.

**Note**  
You cannot grant permissions to a user who has created any applications\. If any such applications exist, the user must delete them before you can grant permissions\.

The following procedures show how to grant **Projects** permissions after you onboard to Studio\. For more information about onboarding to Studio, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

**To grant **Projects** permissions for the administrator and domain execution role users**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel**\.

1. If you choose **Quick setup** to set up your SageMaker Domain, you have permissions to use project templates by default\.

1. If you choose **Standard setup** to set up your SageMaker Domain, make sure you turn on both **Enable Projects, and JumpStart templates for this Studio user** and **Enable Projects, and JumpStart templates for Studio users** when you configure Studio settings\.

1. To confirm that your SageMaker Domain has active project template permissions:

   1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

   1. Choose **Control Panel**\.

   1. Choose the **Settings** icon in the upper\-right corner of the **Domain** card\.

   1. Choose **Studio Settings** in the left side panel\.

   1. Under **Projects and JumpStart**, make sure **Enable Projects, and JumpStart templates for this Studio user** and **Enable Projects, and JumpStart templates for Studio users** are turned on\.