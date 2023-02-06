# SageMaker MLOps Project Walkthrough Using Third\-party Git Repos<a name="sagemaker-projects-walkthrough-3rdgit"></a>

This walkthrough uses the template [MLOps template for model building, training, and deployment with third\-party Git repositories using CodePipeline](sagemaker-projects-templates-sm.md#sagemaker-projects-templates-git-code-pipeline) to demonstrate how to use MLOps projects to create a CI/CD system to build, train, and deploy models\.

**Prerequisites**

To complete this walkthrough, you need:
+ An IAM or IAM Identity Center account to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Permission to use SageMaker\-provided project templates\. For information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.
+ Basic familiarity with the Studio user interface\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.
+ Two GitHub repositories initialized with a README\. You input these repositories into the project template, which will seed these repos with model build and deploy code\.

**Topics**
+ [Step 1: Set up the GitHub connection](#sagemaker-proejcts-walkthrough-connect-3rdgit)
+ [Step 2: Create the Project](#sagemaker-proejcts-walkthrough-create-3rdgit)
+ [Step 3: Make a Change in the Code](#sagemaker-projects-walkthrough-change-3rdgit)
+ [Step 4: Approve the Model](#sagemaker-proejcts-walkthrough-approve-3rdgit)
+ [\(Optional\) Step 5: Deploy the Model Version to Production](#sagemaker-projects-walkthrough-prod-3rdgit)
+ [Step 6: Clean Up Resources](#sagemaker-projectcts-walkthrough-cleanup-3rdgit)

## Step 1: Set up the GitHub connection<a name="sagemaker-proejcts-walkthrough-connect-3rdgit"></a>

In this step, you connect to your GitHub repositories using an [AWS CodeStar connection](https://docs.aws.amazon.com/dtconsole/latest/userguide/welcome-connections.html)\. The SageMaker project uses this connection to access your source code repositories\.

**To set up the GitHub connection:**

1. Log in to the CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline/](https://console.aws.amazon.com/codepipeline/)

1. Under **Settings** in the navigation pane, choose **Connections**\.

1. Choose **Create connection**\.

1. For **Select a provider**, select **GitHub**\.

1. For **Connection name**, enter a name\.

1. Choose **Connect to GitHub**\.

1. If the AWS Connector GitHub app isn’t previously installed, choose **Install new app**\.

   This displays a list of all the GitHub personal accounts and organizations to which you have access\.

1. Choose the account where you want to establish connectivity for use with SageMaker projects and GitHub repositories\.

1. Choose **Configure**\.

1. You can optionally select your specific repositories or choose **All repositories**\.

1. Choose **Save**\. When the app is installed, you’re redirected to the **Connect to GitHub** page and the installation ID is automatically populated\.

1. Choose **Connect**\.

1. Add a tag with the key `sagemaker` and value `true` to this AWS CodeStar connection\.

1. Copy the connection ARN to save for later\. You use the ARN as a parameter in the project creation step\.

## Step 2: Create the Project<a name="sagemaker-proejcts-walkthrough-create-3rdgit"></a>

In this step, you create a SageMaker MLOps project by using a SageMaker\-provided project template to build, train, and deploy models\.

**To create the SageMaker MLOps project**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Choose **Create project**\.

   The **Create project** tab appears\.

1. For **SageMaker project templates**, choose **MLOps template for model building, training, and deployment with third\-party Git repositories**\.

1. Choose **Select project template**\.

1. Under **ModelBuild CodeRepository Info**, provide the following parameters:
   + For **URL**, enter the URL of your Git repository for the model build code in https://*git\-url*\.git format\.
   + For **Branch**, enter the branch to use from your Git repository for pipeline activities\.
   + For **Full Repository Name**, enter the Git repository name in the format of *username/repository name* or *organization/repository name*\.
   + For **Codestar Connection ARN**, enter the ARN of the AWS CodeStar connection you created in Step 1\.
   + The **Sample Code** toggle switch lets you choose whether to populate the repository with model build seed code\. We can leave it on for this demo\.

1. Under **ModelDeploy CodeRepository Info**, provide the following parameters:
   + For **URL**, enter the URL of your Git repository for the model deploy code in https://*git\-url*\.git format\.
   + For **Branch**, enter the branch to use from your Git repository for pipeline activities\.
   + For **Full Repository Name**, enter the Git repository name in the format of *username/repository name* or *organization/repository name*\.
   + For **Codestar Connection ARN**, enter the ARN of the AWS CodeStar connection you created in Step 1\.
   + The **Sample Code** toggle switch lets you choose whether to populate the repository with model deployment seed code\. We can leave it on for this demo\.

1. Choose **Create Project**\.

The project appears in the **Projects** list with a **Status** of **Created**\.

## Step 3: Make a Change in the Code<a name="sagemaker-projects-walkthrough-change-3rdgit"></a>

Now make a change to the pipeline code that builds the model and commit the change to initiate a new pipeline run\. The pipeline run registers a new model version\.

**To make a code change**

1. In your model build GitHub repo, navigate to the `pipelines/abalone` folder\. Double\-click `pipeline.py` to open the code file\.

1. In the `pipeline.py` file, find the line that sets the training instance type\.

   ```
   training_instance_type = ParameterString(
           name="TrainingInstanceType", default_value="ml.m5.xlarge"
   ```

   Open the file for editing, change `ml.m5.xlarge` to `ml.m5.large`, then commit\.

After you commit your code change, the MLOps system initiates a run of the pipeline that creates a new model version\. In the next step, you approve the new model version to deploy it to production\.

## Step 4: Approve the Model<a name="sagemaker-proejcts-walkthrough-approve-3rdgit"></a>

Now you approve the new model version that was created in the previous step to initiate a deployment of the model version to a SageMaker endpoint\.

**To approve the model version**

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Find the name of the project you created in the first step and double\-click on it to open the project tab for your project\.

1. In the project tab, choose **Model groups**, then double\-click the name of the model group that appears\.

   The model group tab appears\.

1. In the model group tab, double\-click **Version 1**\. The **Version 1** tab opens\. Choose **Update status**\.

1. In the model **Update model version status** dialog box, in the **Status** dropdown list, select **Approve** and then choose **Update status**\.

   Approving the model version causes the MLOps system to deploy the model to staging\. To view the endpoint, choose the **Endpoints** tab on the project tab\.

## \(Optional\) Step 5: Deploy the Model Version to Production<a name="sagemaker-projects-walkthrough-prod-3rdgit"></a>

Now you can deploy the model version to the production environment\.

**Note**  
To complete this step, you need to be an administrator in your Studio domain\. If you are not an administrator, skip this step\.

**To deploy the model version to the production environment**

1. Log in to the CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline/](https://console.aws.amazon.com/codepipeline/)

1. Choose **Pipelines**, then choose the pipeline with the name **sagemaker\-*projectname*\-*projectid*\-modeldeploy**, where *projectname* is the name of your project, and *projectid* is the ID of your project\.

1. In the **DeployStaging** stage, choose **Review**\.

1. In the **Review** dialog box, choose **Approve**\.

   Approving the **DeployStaging** stage causes the MLOps system to deploy the model to production\. To view the endpoint, choose the **Endpoints** tab on the project tab in Studio\.

## Step 6: Clean Up Resources<a name="sagemaker-projectcts-walkthrough-cleanup-3rdgit"></a>

To stop incurring charges, clean up the resources that were created in this walkthrough\.

**Note**  
To delete the AWS CloudFormation stack and the Amazon S3 bucket, you need to be an administrator in Studio\. If you are not an administrator, ask your administrator to complete those steps\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Select the target project from the dropdown list\. If you don’t see your project, type the project name and apply the filter to find your project\.

1. Select your project to view its details in the main panel\.

1. Choose **Delete** from the **Actions** menu\.

1. Confirm your choice by choosing **Delete** from the **Delete Project** window\.

   This deletes the Service Catalog provisioned product that the project created\. This includes the CodeCommit, CodePipeline, and CodeBuild resources created for the project\.

1. Delete the AWS CloudFormation stacks that the project created\. There are two stacks, one for staging and one for production\. The names of the stacks are **sagemaker\-*projectname*\-*project\-id*\-deploy\-staging** and **sagemaker\-*projectname*\-*project\-id*\-deploy\-prod**, where *projectname* is the name of your project, and *project\-id* is the ID of your project\.

   For information about how to delete a AWS CloudFormation stack, see [Deleting a stack on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) in the *AWS CloudFormation User Guide*\.

1. Delete the Amazon S3 bucket that the project created\. The name of the bucket is **sagemaker\-project\-*project\-id***, where *project\-id* is the ID of your project\.