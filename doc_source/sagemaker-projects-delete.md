# Delete an MLOps Project using Amazon SageMaker Studio<a name="sagemaker-projects-delete"></a>

This procedure demonstrates how to delete an MLOps project using Amazon SageMaker Studio\.

**Prerequisites**

**Note**  
You can only delete projects in Studio that you have created\. This condition is part of the service catalog permission `servicecatalog:TerminateProvisionedProduct` in the `AmazonSageMakerFullAccess` policy\. If needed, you can update this policy to remove this condition\.
+ An IAM account or IAM Identity Center to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**To delete a project in Amazon SageMaker Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Select the target project from the dropdown list\. If you don’t see your project, type the project name and apply the filter to find your project\.

1. Once you've found your project, select the project name to view details\.

1. Choose **Delete** from the **Actions** menu\.

1. Confirm your choice by choosing **Delete** from the **Delete Project** window\.