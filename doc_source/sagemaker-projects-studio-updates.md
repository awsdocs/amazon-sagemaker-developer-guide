# SageMaker Studio Permissions Required to Use Projects<a name="sagemaker-projects-studio-updates"></a>

Users can view SageMaker provided project templates and create projects with those templates when you grant **Projects** permissions for users\. You can grant these permissions when you onboard or update Amazon SageMaker Studio\. There are two permissions to grant\.

1. Grant **Projects** permissions for the Studio administrator to permit the Studio administrator to view the SageMaker\-provided templates in the Service Catalog console\. The administrator can see what other Studio users create if you grant them permission to use SageMaker projects\. The administrator can also view the AWS CloudFormation template that the SageMaker\-provided project templates define in the Service Catalog console\. For information about using the Service Catalog console, see [What Is Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) in the *Service Catalog User Guide*\.

1. Allow Studio users who are configured to use the same execution role as the domain to create projects\. This grants Studio users permission to use the SageMaker\-provided project templates to create a project from within Studio\.

**Important**  
Do not manually create your roles\. Always create roles through **Studio Settings** using the steps described in the following procedure\.

For users who use any role other than the domain's execution role to view and use SageMaker\-provided project templates, you need to grant **Projects** permissions to the individual user profiles\.

The following procedures show how to grant **Projects** permissions after you onboard to Studio\. For more information about onboarding to Studio, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

**To grant **Projects** permissions for the administrator and domain execution role users**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel**\.

1. If you choose **Quick setup** to set up your SageMaker Domain, you have permissions to use project templates by default\.

1. If you choose **Standard setup** to set up your SageMaker Domain, make sure you turn on the following options when you configure Studio settings:
   + **Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for this account**
   + **Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for Studio users**
   + **Create the roles which are needed to use the latest updated AWS Service catalog of products for Projects and JumpStart**

1. To confirm that your SageMaker Domain has active project template permissions:

   1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

   1. Choose **Control Panel**\.

   1. Choose the **Settings** icon in the upper\-right corner of the **Domain** card\.

   1. Choose **Studio Settings** in the left side panel\.

   1. Under **Projects and JumpStart**, make sure the following options are turned on:
      + **Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for this account**
      + **Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for Studio users**
      + **Create the roles which are needed to use the latest updated AWS Service catalog of products for Projects and JumpStart**

1. To view a list of your roles:

   1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

   1. Choose **Control Panel**\.

      A list of your roles appears in the `Apps` card under `Projects`\.
**Important**  
As of July 25, we require additional roles to use project templates\. Here is the complete list of roles you should see under `Projects`:  
`AmazonSageMakerServiceCatalogProductsLaunchRole` `AmazonSageMakerServiceCatalogProductsUseRole` `AmazonSageMakerServiceCatalogProductsApiGatewayRole` `AmazonSageMakerServiceCatalogProductsCloudformationRole` `AmazonSageMakerServiceCatalogProductsCodeBuildRole` `AmazonSageMakerServiceCatalogProductsCodePipelineRole` `AmazonSageMakerServiceCatalogProductsEventsRole` `AmazonSageMakerServiceCatalogProductsFirehoseRole` `AmazonSageMakerServiceCatalogProductsGlueRole` `AmazonSageMakerServiceCatalogProductsLambdaRole` `AmazonSageMakerServiceCatalogProductsExecutionRole`