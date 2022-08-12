# Launch a Solution<a name="jumpstart-solutions-launch"></a>

On the page for each solution, JumpStart shows a description of the solution and a `Launch` button\. To launch a solution, select `Launch`\. JumpStart then creates all of the resources needed to run the solution\. This includes training and model hosting instances\. After you choose a solution, the **Launch Solution** pane opens\.

## Advanced parameters<a name="jumpstart-solutions-config"></a>

The solution that you select may have advanced parameters that you can select\. Choose **Advanced Parameters** to specify the AWS Identity and Access Management role for the solution\. 

Solutions are able to launch resources across 9 AWS services that interact with each other\. For the solution to work as expected, newly created components from one service must be able to act on newly created components from another service\. We recommend that you use the default IAM role to ensure that all needed permissions are added\. For more information about IAM roles, see [Identity and Access Management for Amazon SageMaker](security-iam.md)\.

**Default IAM role**

If you select this option, the default IAM roles that are required by this solution are used\. Each solution requires different resources\. The following list describes the default roles that are used for the solutions based on the service needed\. For a description of the permissions required for each service, see [AWS Managed Policies for SageMaker projects and JumpStart](security-iam-awsmanpol-sc.md)\.
+ **API Gateway** – AmazonSageMakerServiceCatalogProductsApiGatewayRole 
+ **CloudFormation** – AmazonSageMakerServiceCatalogProductsCloudformationRole
+ **CodeBuild** – AmazonSageMakerServiceCatalogProductsCodeBuildRole 
+ **CodePipeline** – AmazonSageMakerServiceCatalogProductsCodePipelineRole
+ **Events** – AmazonSageMakerServiceCatalogProductsEventsRole
+ **Firehose** – AmazonSageMakerServiceCatalogProductsFirehoseRole
+ **Glue** – AmazonSageMakerServiceCatalogProductsGlueRole
+ **Lambda** – AmazonSageMakerServiceCatalogProductsLambdaRole
+ **SageMaker** – AmazonSageMakerServiceCatalogProductsExecutionRole 

If you are using a new SageMaker domain with JumpStart project templates enabled, these roles are automatically created in your account\.

If you are using an existing SageMaker domain, these roles may not exist in your account\. If this is the case, you will receive the following error when launching the solution\. 

```
Unable to locate the updated roles required to launch this solution, a general role '/service-role/AmazonSageMakerServiceCatalogProductsUseRole' will be used. Please update your studio domain to generate these roles.
```

You can still launch a solution without the needed role, but the legacy default role `AmazonSageMakerServiceCatalogProductsUseRole` is used in place of the needed role\. The legacy default role has trust relationships with all of the services that JumpStart solutions need to interact with\. For the best security, we recommend that you update your domain to have the newly created default roles for each AWS service\.

If you have already onboarded to a SageMaker domain, you can update your domain to generate the default roles using the following procedure\.

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose ** Control Panel ** at the top left of the page\.

1. From the **Domain** page, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) to edit the domain settings\.

1. On **General Settings** choose **Next**\.

1. Under **SageMaker Projects and JumpStart**, select **Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for this account ** and **Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for Studio users**, choose **Next**\.

1. Select **Submit**\.

You should be able to see the default roles listed in **Projects \- Amazon SageMaker project templates enabled for this account** under the **Apps \- Studio** tab\.

**Find IAM role**

If you select this option, you must select an existing IAM role from the dropdown list for each of the required services\. The selected role must have at least the minimum permissions required for the corresponding service\. For a description of the permissions required for each service, see [AWS Managed Policies for SageMaker projects and JumpStart](security-iam-awsmanpol-sc.md)\.

**Input IAM role**

If you select this option, you must manually enter the ARN for an existing IAM role\. The selected role must have at least the minimum permissions required for the corresponding service\. For a description of the permissions required for each service, see [AWS Managed Policies for SageMaker projects and JumpStart](security-iam-awsmanpol-sc.md)\.