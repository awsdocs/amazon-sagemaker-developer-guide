# Update an MLOps Project in Amazon SageMaker Studio<a name="sagemaker-projects-update"></a>

This procedure demonstrates how to update an MLOps project in Amazon SageMaker Studio\. You can update the **Description**, template version, and template parameters\.

**Prerequisites**
+ An IAM account or IAM Identity Center to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.
+ Add the following custom inline policies to the specified roles:

  User\-created role having `AmazonSageMakerFullAccess`

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "servicecatalog:CreateProvisionedProductPlan",
                  "servicecatalog:DescribeProvisionedProductPlan",
                  "servicecatalog:DeleteProvisionedProductPlan"
              ],
              "Resource": "*"
          }
      ]
  }
  ```

  `AmazonSageMakerServiceCatalogProductsLaunchRole`

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "cloudformation:CreateChangeSet",
                  "cloudformation:DeleteChangeSet",
                  "cloudformation:DescribeChangeSet"
              ],
              "Resource": "arn:aws:cloudformation:*:*:stack/SC-*"
          },
          {
              "Effect": "Allow",
              "Action": [
                  "codecommit:PutRepositoryTriggers"
              ],
              "Resource": "arn:aws:codecommit:*:*:sagemaker-*"
          }
      ]
  }
  ```

**To update a project in Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\. A list of your projects appears\.

1. Select the name of the project you want to update in the projects list\.

1. Choose **Update** from the **Actions** menu in the upper\-right corner of the project tab\.

1. In the **Update project** dialog box, you can edit the **Description** and listed template parameters\.

1. Choose **View difference**\.

   A dialog box displays your original and updated project settings\. Any change in your project settings can modify or delete resources in the current project\. The dialog box displays these changes as well\.

1. You may need to wait a few minutes for the **Update** button to become active\. Choose **Update**\.

1. The project update may take a few minutes to complete\. Select **Settings** in the project tab and ensure the parameters have been updated correctly\.