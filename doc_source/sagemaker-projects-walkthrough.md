# SageMaker MLOps Project Walkthrough<a name="sagemaker-projects-walkthrough"></a>

This walkthrough uses the template [MLOps template for model building, training, and deployment](sagemaker-projects-templates-sm.md#sagemaker-projects-templates-code-commit) to demonstrate using MLOps projects to create a CI/CD system to build, train, and deploy models\.

**Prerequisites**

To complete this walkthrough, you need:
+ An IAM account or IAM Identity Center to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Permission to use SageMaker\-provided project templates\. For information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [Step 1: Create the Project](#sagemaker-proejcts-walkthrough-create)
+ [Step 2: Clone the Code Repository](#sagemaker-proejcts-walkthrough-clone)
+ [Step 3: Make a Change in the Code](#sagemaker-projects-walkthrough-change)
+ [Step 4: Approve the Model](#sagemaker-proejcts-walkthrough-approve)
+ [\(Optional\) Step 5: Deploy the Model Version to Production](#sagemaker-projects-walkthrough-prod)
+ [Step 6: Clean Up Resources](#sagemaker-projectcts-walkthrough-cleanup)

## Step 1: Create the Project<a name="sagemaker-proejcts-walkthrough-create"></a>

In this step, you create a SageMaker MLOps project by using a SageMaker\-provided project template to build, train, and deploy models\.

**To create the SageMaker MLOps project**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Choose **Create project**\.

   The **Create project** tab appears\.

1. If not selected already, choose **SageMaker templates**, then choose **MLOps template for model building, training, and deployment**\.

1. For **Project details**, enter a name and description for your project\.

When the project appears in the **Projects** list with a **Status** of **Create completed**, move on to the next step\.

**Important**  
As of July 25, 2022, we require additional roles to use project templates\. If you see the error message **CodePipeline is not authorized to perform AssumeRole on role arn:aws:iam::xxx:role/service\-role/AmazonSageMakerServiceCatalogProductsCodePipelineRole**, see Steps 5\-6 of [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md) for a complete list of required roles and instructions on how to create them\.

## Step 2: Clone the Code Repository<a name="sagemaker-proejcts-walkthrough-clone"></a>

After you create the project, two CodeCommit repositories are created in the project\. One of the repositories contains code to build and train a model, and one contains code to deploy the model\. In this step, you clone the repository to your local SageMaker project that contains the code to build and train the model to the local Studio environment so that you can work with the code\.

**To clone the code repository**

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Select the project you created in the previous step to open the project tab for your project\.

1. In the project tab, choose **Repositories**, and in the **Local path** column for the repository that ends with **modelbuild**, choose **clone repo\.\.\.**\.

1. In the dialog box that appears, accept the defaults and choose **Clone repository**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-clone-details.png)

   When clone of the repository is complete, the local path appears in the **Local path** column\. Choose the path to open the local folder that contains the repository code in Studio\.

## Step 3: Make a Change in the Code<a name="sagemaker-projects-walkthrough-change"></a>

Now make a change to the pipeline code that builds the model and check in the change to initiate a new pipeline run\. The pipeline run registers a new model version\.

**To make a code change**

1. In Studio, choose the file browser icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \), and navigate to the `pipelines/abalone` folder\. Double\-click `pipeline.py` to open the code file\.

1. In the `pipeline.py` file, find the line that sets the training instance type\.

   ```
   training_instance_type = ParameterString(
           name="TrainingInstanceType", default_value="ml.m5.xlarge"
   ```

   Change `ml.m5.xlarge` to `ml.m5.large`, then type `Ctrl+S` to save the change\.

1. Choose the **Git** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid.png) \)\. Stage, commit, and push the change in `pipeline.py`\. Also, enter a summary in the **Summary** field and an optional description in the **Description** field\. For information about using Git in Studio, see [Clone a Git Repository in SageMaker Studio](studio-tasks-git.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/projects-walkthrough-commit.png)

After pushing your code change, the MLOps system initiates a run of the pipeline that creates a new model version\. In the next step, you approve the new model version to deploy it to production\.

## Step 4: Approve the Model<a name="sagemaker-proejcts-walkthrough-approve"></a>

Now you approve the new model version that was created in the previous step to initiate a deployment of the model version to a SageMaker endpoint\.

**To approve the model version**

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Select the name of the project you created in the first step to open the project tab for your project\.

1. In the project tab, choose **Model groups**, then double\-click the name of the model group that appears\.

   The model group tab appears\.

1. In the model group tab, double\-click **Version 1**\. The **Version 1** tab opens\. Choose **Update status**\.

1. In the model **Update model version status** dialog box, in the **Status** dropdown list, select **Approve**, then choose **Update status**\.

   Approving the model version causes the MLOps system to deploy the model to staging\. To view the endpoint, choose the **Endpoints** tab on the project tab\.

## \(Optional\) Step 5: Deploy the Model Version to Production<a name="sagemaker-projects-walkthrough-prod"></a>

Now you can deploy the model version to the production environment\.

**Note**  
To complete this step, you need to be an administrator in your Studio domain\. If you are not an administrator, skip this step\.

**To deploy the model version to the production environment**

1. Log in to the CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline/](https://console.aws.amazon.com/codepipeline/)

1. Choose **Pipelines**, then choose the pipeline with the name **sagemaker\-*projectname*\-*projectid*\-modeldeploy**, where *projectname* is the name of your project, and *projectid* is the ID of your project\.

1. In the **DeployStaging** stage, choose **Review**\.

1. In the **Review** dialog box, choose **Approve**\.

   Approving the **DeployStaging** stage causes the MLOps system to deploy the model to production\. To view the endpoint, choose the **Endpoints** tab on the project tab in Studio\.

## Step 6: Clean Up Resources<a name="sagemaker-projectcts-walkthrough-cleanup"></a>

To stop incurring charges, clean up the resources that were created in this walkthrough\. To do this, complete the following steps\.

**Note**  
To delete the AWS CloudFormation stack and the Amazon S3 bucket, you need to be an administrator in Studio\. If you are not an administrator, ask your administrator to complete those steps\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Select the target project from the dropdown list\. If you don’t see your project, type the project name and apply the filter to find your project\.

1. 

**You can delete a Studio project in one of the following ways:**

   1. 

**You can delete the project from the projects list\.**

      Right\-click the target project and choose ** Delete** from the dropdown list\.
**Note**  
This functionality is supported in Studio version v3\.17\.1 or higher\. For more information, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

   1. 

**You can delete a project from the **Project details** section\.**

      1. When you've found your project, double\-click it to view its details in the main panel\.

      1. Choose **Delete** from the **Actions** menu\.

1. Confirm your choice by choosing **Delete** from the **Delete Project** window\.

   This deletes the Service Catalog provisioned product that the project created\. This includes the CodeCommit, CodePipeline, and CodeBuild resources created for the project\.

1. Delete the AWS CloudFormation stacks that the project created\. There are two stacks, one for staging and one for production\. The names of the stacks are **sagemaker\-*projectname*\-*project\-id*\-deploy\-staging** and **sagemaker\-*projectname*\-*project\-id*\-deploy\-prod**, where *projectname* is the name of your project, and *project\-id* is the ID of your project\.

   For information about how to delete a AWS CloudFormation stack, see [Deleting a stack on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) in the *AWS CloudFormation User Guide*\.

1. Delete the Amazon S3 bucket that the project created\. The name of the bucket is **sagemaker\-project\-*project\-id***, where *project\-id* is the ID of your project\.