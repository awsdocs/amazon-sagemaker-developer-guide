# Delete an MLOps Project using Amazon SageMaker Studio<a name="sagemaker-projects-delete"></a>

This procedure demonstrates how to delete an MLOps project using Amazon SageMaker Studio\.

**Prerequisites**

**Note**  
You can only delete projects in Studio that you have created\. This condition is part of the service catalog permission `servicecatalog:TerminateProvisionedProduct` in the `AmazonSageMakerFullAccess` policy\. If needed, you can update this policy to remove this condition\.
+ An AWS SSO or IAM account to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**To delete a project in Amazon SageMaker Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **SageMaker resources** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png)\)\.

1. Select **Projects** from the dropdown list\.

1. Select the target project from the dropdown list\. If you don’t see your project, type the project name and apply the filter to find your project\.

1. 

**You can delete a Studio project in one of the following ways:**

   1. 

**You can delete the project from the projects list\.**

      Right\-click the target project and choose ** Delete** from the dropdown list\.
**Note**  
This functionality is supported in Studio version v3\.17\.1 or higher\. For more information, see [Update SageMaker Studio](studio-tasks-update-studio.md)\.

   1. 

**You can delete a project from the **Project details** section\.**

      1. Once you've found your project, double\-click it to view its details\.

      1. Choose **Delete** from the **Actions** menu\.

1. Confirm your choice by choosing **Delete** from the **Delete Project** window\.