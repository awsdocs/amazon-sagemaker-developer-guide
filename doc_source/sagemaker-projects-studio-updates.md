# SageMaker Studio Permissions Required to Use Projects<a name="sagemaker-projects-studio-updates"></a>

To enable users to view SageMaker provided project templates and create projects with those templates, enable **Projects** permissions for users when you onboard or update SageMaker Studio\. There are 2 permissions to enable\.

1. Enable **Projects** permissions for the SageMaker Studio administrator\. This enables the SageMaker Studio administrator to view the SageMaker provided templates in the AWS Service Catalog console\. The administrator can see what other SageMaker Studio users create if you grant them permission to use SageMaker projects\. The administrator can also view the AWS CloudFormation template that the SageMaker provided project templates define in the Service Catalog Console\. For information about using the Service Catalog console, see [What Is AWS Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) in the *AWS Service Catalog User Guide*\.

1. Enable projects for SageMaker studio users who are configured to use the domain execution role\. This grants permission for SageMaker Studio users the ability to use the SageMaker provided project templates to create a project from within SageMaker Studio\.

To enable users who use any role other than the domain execution role to view and use SageMaker provided project templates, you need to enable **Projects** permissions on the individual user profiles\.

The following procedures show how to enable **Projects** permissions when you are onboarding to SageMaker Studio\. For more information about onboarding to SageMaker Studio, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

**To enable **Projects** permissions for the administrator and domain execution role users**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Amazon SageMaker Studio** at the top left of the page\.

1. If you choose **Quick start**, under **Projects**, make sure **Enable SageMaker project templates for this account and Studio users** is enabled\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/project-permission-quickstart.png)

1. If you choose **Standard setup**, under **Projects**, make sure both **Enable SageMaker project templates for this account** and **Enable SageMaker project templates for Studio users** are enabled\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/project-permission-standard.png)