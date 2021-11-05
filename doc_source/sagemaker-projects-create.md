# Create an MLOps Project using Amazon SageMaker Studio<a name="sagemaker-projects-create"></a>

This procedure demonstrates how to create an MLOps project using Amazon SageMaker Studio\.

**Prerequisites**
+ An AWS SSO or IAM account to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Permission to use SageMaker\-provided project templates\. For information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**To create a project in Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **SageMaker resources** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png)\)\.

1. Select **Projects** from the dropdown list\.

1. Choose **Create project**\.

   The **Create project** tab opens displaying a list of available templates\.

1. For **SageMaker project templates**, choose **SageMaker templates**\. For more information about project templates, see [MLOps Project Templates](sagemaker-projects-templates.md)\.

1. Choose **MLOps template for model building, training, and deployment**\.

1. Choose **Select project template**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-template.png)

   The **Create project** tab changes to display **Project details**\.

1. Enter the following information:
   + For **Project details**, enter a name and description for your project\.
   + Optionally, add tags, which are key\-value pairs that you can use to track your projects\.

1. Choose **Create project** and wait for the project to appear in the **Projects** list\.