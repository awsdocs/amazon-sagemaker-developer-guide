# View Project Resources<a name="sagemaker-projects-resources"></a>

After you create a project, view the resources associated with the project in Amazon SageMaker Studio\.

**To create a project in Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Deployments** from the menu, and then select **Projects**\.

1. Select the name of the project for which you want to view details\.

   A tab with the project details appears\.

On the project details tab, you can view the following entities associated with the project\.
+ Repositories: Code repositories \(repos\) associated with this project\. If you use a SageMaker\-provided template when you create your project, it creates a AWS CodeCommit repo or a third\-party Git repo\. For more information about CodeCommit, see [What is AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html)\.
+ Pipelines: SageMaker ML pipelines that define steps to prepare data, train, and deploy models\. For information about SageMaker ML pipelines, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.
+ Experiments: One or more Amazon SageMaker Autopilot experiments associated with the project\. For information about Autopilot, see [Automate model development with Amazon SageMaker Autopilot](autopilot-automate-model-development.md)\.
+ Model groups: Groups of model versions that were created by pipeline executions in the project\. For information about model groups, see [Create a Model Group](model-registry-model-group.md)\.
+ Endpoints: SageMaker endpoints that host deployed models for real\-time inference\. When a model version is approved, it is deployed to an endpoint\.
+ Settings: Settings for the project\. This includes the name and description of the project, information about the project template and `SourceModelPackageGroupName`, and metadata about the project\.