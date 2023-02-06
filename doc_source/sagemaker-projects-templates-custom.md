# Create Custom Project Templates<a name="sagemaker-projects-templates-custom"></a>

If the SageMaker\-provided templates do not meet your needs \(for example, you want to have more complex orchestration in the CodePipeline with multiple stages or custom approval steps\), create your own templates\.

We recommend starting by using SageMaker\-provided templates to understand how to organize your code and resources and build on top of it\. To do this, after you enable administrator access to the SageMaker templates, log in to the [https://console\.aws\.amazon\.com/servicecatalog/](https://console.aws.amazon.com/servicecatalog/), choose **Portfolios**, then choose **Imported**\. For information about Service Catalog, see [Overview of Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/what-is_concepts.html) in the *Service Catalog User Guide*\.

Create your own project templates to customize your MLOps project\. SageMaker project templates are Service Catalog–provisioned products to provision the resources for your MLOps project\. 

To create a custom project template, complete the following steps\.

1. Create a portfolio\. For information, see [Step 3: Create an Service Catalog Portfolio](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/getstarted-portfolio.html)\.

1. Create a product\. A product is a CloudFormation template\. You can create multiple versions of the product\. For information, see [Step 4: Create an Service Catalog Product](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/getstarted-product.html)\.

   For the product to work with SageMaker projects, add the following parameters to your product template\.

   ```
   SageMakerProjectName:
   Type: String
   Description: Name of the project
   
   SageMakerProjectId:
   Type: String
   Description: Service generated Id of the project.
   ```
**Important**  
We recommend that you wrap the CodeCommit repository into the SageMaker code repository for the project's repositories to be visible in VPC mode\. The sample template and required addition are shown in the following code samples\.  
Original \(sample\) template:  

   ```
   ModelBuildCodeCommitRepository:
       Type: AWS::CodeCommit::Repository
       Properties:
         # Max allowed length: 100 chars
         RepositoryName: !Sub sagemaker-${SageMakerProjectName}-${SageMakerProjectId}-modelbuild # max: 10+33+15+10=68
         RepositoryDescription: !Sub SageMaker Model building workflow infrastructure as code for the Project ${SageMakerProjectName}
         Code:
           S3:
             Bucket: SEEDCODE_BUCKETNAME
             Key: toolchain/model-building-workflow-v1.0.zip
           BranchName: main
   ```
Additional content to add in VPC mode:  

   ```
   SageMakerRepository:
       Type: AWS::SageMaker::CodeRepository
       Properties:
           GitConfig:
               RepositoryUrl: !GetAtt ModelBuildCodeCommitRepository.CloneUrlHttp
               Branch: main
   ```

1. Add a launch constraint\. A launch constraint designates an IAM role that Service Catalog assumes when a user launches a product\. For information, see [Step 6: Add a Launch Constraint to Assign an IAM Role](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/getstarted-launchconstraint.html)\.

1. Provision the product on [https://console\.aws\.amazon\.com/servicecatalog/](https://console.aws.amazon.com/servicecatalog/) to test the template\. If you are satisfied with your template, continue to the next step to make the template available in Studio\.

1. Grant access to the Service Catalog portfolio that you created in step 1 to your Studio execution role\. Use either the Studio domain execution role or a user role that has Studio access\. For information about adding a role to the portfolio, see [Step 7: Grant End Users Access to the Portfolio](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/getstarted-deploy.html)\.

1. To make your project template available in your **Organization templates** list in Studio, create a tag with the following key and value to the Service Catalog product you created in step 2\.
   + **key**: `sagemaker:studio-visibility`
   + **value**: `true`

After you complete these steps, Studio users in your organization can create a project with the template you created by following the steps in [Create an MLOps Project using Amazon SageMaker Studio](sagemaker-projects-create.md) and choosing **Organization templates** when you choose a template\.