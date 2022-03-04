# Use SageMaker\-Provided Project Templates<a name="sagemaker-projects-templates-sm"></a>

Amazon SageMaker provides project templates that create the infrastructure you need to create an MLOps solution for continuous integration and continuous deployment \(CI/CD\) of ML models\. Use these templates to process data, extract features, train and test models, register the models in the SageMaker model registry, and deploy the models for inference\. You can customize the seed code and the configuration files to suit your requirements\.

**Important**  
As of July 27, 2021, SageMaker projects can use third\-party Git repositories\. For more information, see [Update SageMaker Projects to Use Third\-Party Git Repositories](#sagemaker-projects-templates-update)\.

SageMaker project templates offer you the following choice of code repositories, workflow automation tools, and pipeline stages:
+ **Code repository**: AWS CodeCommit or third\-party Git repositories such as GitHub and Bitbucket
+ **CI/CD workflow automation**: AWS CodePipeline or Jenkins
+ **Pipeline stages**: Model building and training, model deployment, or both

Each of the following topics corresponds to a template you can choose when you create your SageMaker project\. You can view the available templates in [ Step 1: Create the Project](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-projects-walkthrough.html#sagemaker-proejcts-walkthrough-create) of the [Project walkthrough](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-projects-walkthrough.html)\.

**Topics**
+ [MLOps template for model building, training, and deployment](#sagemaker-projects-templates-code-commit)
+ [MLOps template for model building, training, deployment, and Amazon SageMaker Model Monitor](#sagemaker-projects-templates-model-building-training-deployment-model-monitor)
+ [MLOps template for image building, model building, and model deployment](#sagemaker-projects-templates-image-building-model-building-deployment)
+ [MLOps template for model building, training, and deployment with third\-party Git repositories using CodePipeline](#sagemaker-projects-templates-git-code-pipeline)
+ [MLOps template for model building, training, and deployment with third\-party Git repositories using Jenkins](#sagemaker-projects-templates-git-jenkins)
+ [Update SageMaker Projects to Use Third\-Party Git Repositories](#sagemaker-projects-templates-update)

## MLOps template for model building, training, and deployment<a name="sagemaker-projects-templates-code-commit"></a>

This template is a combination of the following two templates, each of which can be used independently, and contains all of the resources provided in those templates\.
+ **Code repository**: AWS CodeCommit
+ **CI/CD workflow automation**: AWS CodePipeline

**MLOps template for model building and training**  
Use this template when you want an MLOps solution to process data, extract features, train and test models, and register the models in the SageMaker model registry\.  
This template provides the following resources:  
+ An AWS CodeCommit repository that contains sample code that creates an Amazon SageMaker Model Building Pipelines pipeline in Python code and shows how to create and update the SageMaker pipeline\. This repository also has a Python Jupyter Notebook that you can open and run in Studio\.
+ An AWS CodePipeline pipeline that has source and build steps\. The source step points to the CodeCommit repository\. The build step gets the code from that repository, creates and updates the SageMaker pipeline, starts a pipeline execution, and waits for the pipeline execution to complete\.
+ An Amazon S3 bucket to store artifacts, including CodePipeline and CodeBuild artifacts, and any artifacts generated from the SageMaker pipeline runs\.
The following diagram illustrates the workflow and AWS resources used by this template to help you build and train your models\.  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/template_build.jpg)

**MLOps template for model deployment**  
Use this template to automate the deployment of models in the SageMaker model registry to SageMaker endpoints for real\-time inference\. This template recognizes changes in the model registry\. When a new model version is registered and approved, it automatically initiates a deployment\.  
The template provisions a CodeCommit repository with configuration files to specify the model deployment steps, AWS CloudFormation templates to define endpoints as infrastructure, and seed code for testing the endpoint\.  
This template provides the following resources:  
+ An AWS CodeCommit repository that contains sample code that deploys models to endpoints in staging and production environments\.
+ An AWS CodePipeline pipeline that has source, build, deploy\-to\-staging, and deploy\-to\-production steps\. The source step points to the CodeCommit repository, and the build step gets the code from that repository and generates CloudFormation stacks to deploy\. The deploy\-to\-staging and deploy\-to\-production steps deploy the CloudFormation stacks to their respective environments\. There is a manual approval step between the staging and production build steps, so that a MLOps engineer must approve the model before it is deployed to production\.

  There is also a programmatic approval step with placeholder tests in the example code in the CodeCommit repository\. You can add additional tests to replace the placeholders tests\.
+ An Amazon S3 bucket to store artifacts, including CodePipeline and CodeBuild artifacts, and any artifacts generated from the SageMaker pipeline runs\.
+ A CloudWatch event to initiate the pipeline when a model package version is approved or rejected\.
The following diagram illustrates the workflow and AWS resources used by this template to help you deploy your models\.  

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/template_deploy.jpg)

## MLOps template for model building, training, deployment, and Amazon SageMaker Model Monitor<a name="sagemaker-projects-templates-model-building-training-deployment-model-monitor"></a>

This template is an extension of the MLOps template for model building, training, and deployment\. It includes both the model building, training, and deployment components of the template, and an additional Amazon SageMaker Model Monitor template that provides the following types of monitoring: 
+ [Data Quality](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-data-quality.html) – Monitor drift in data quality\.
+ [Model Quality](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-model-quality.html) – Monitor drift in model quality metrics, such as accuracy\.
+ [ Bias Drift for Models in Production](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-model-monitor-bias-drift.html) – Monitor bias in a model's predictions\.
+ **Code repository**: AWS CodeCommit
+ **CI/CD workflow automation**: AWS CodePipeline

### MLOps template for Amazon SageMaker Model Monitor<a name="sagemaker-projects-template-model-monitor"></a>

You can use this template for an MLOps solution to deploy one or more of the Amazon SageMaker data quality, model quality, model bias, and model explainability monitors to monitor a deployed model on a SageMaker inference endpoint\. 

This template provides the following resources:
+ An AWS CodeCommit repository that contains sample Python code that gets the [baselines](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-create-baseline.html) used by the monitors from the SageMaker Model Registry, and updates the template’s parameters for the staging and production environments\. It also contains a AWS CloudFormation template to create the Amazon SageMaker Model Monitors\.
+ An AWS CodePipeline pipeline that has source, build, and deploy steps\. The source step points to the CodePipeline repository\. The build step gets the code from that repository, gets the baseline from the Model Registry, and updates template parameters for the staging and production environments\. The deploy steps deploy the configured monitors into the staging and production environments\. The manual approval step, within the `DeployStaging` stage, requires you to verify that the production SageMaker endpoint is `InService` before approving and moving to the `DeployProd` stage\.
+ The template uses the same S3 bucket created by the MLOps template for model building, training, and deployment to store the monitors' outputs\. 
+ Two Amazon EventBridge events rules initiate the Amazon SageMaker Model Monitor AWS CodePipeline every time the staging SageMaker endpoint is updated, or a code change is committed to the CodePipeline repository\. 

## MLOps template for image building, model building, and model deployment<a name="sagemaker-projects-templates-image-building-model-building-deployment"></a>

This template is an extension of the [MLOps template for model building, training, and deployment](#sagemaker-projects-templates-code-commit)\. It includes both the model building, training, and deployment components of that template and the following options:
+ Include processing image–building pipeline
+ Include training image–building pipeline
+ Include inference image–building pipeline

For each of the components selected during project creation, the following are created by using the template:
+ An Amazon ECR repository
+ [A SageMaker Image](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateImage.html)
+ A CodeCommit repository containing a Dockerfile that you can customize
+ A CodePipeline that is initiated by changes to the CodePipeline repository
+ A CodeBuild project that builds a Docker image and registers it in the Amazon ECR repository
+ An EventBridge rule that initiates the CodePipeline on a schedule

When the CodePipeline is initiated, it builds a new Docker container and registers it with an Amazon ECR repository\. When a new container is registered with the Amazon ECR repository, a new `ImageVersion` is added to the SageMaker image\. This initiates the model building pipeline, which in turn initiates the deployment pipeline\.

The newly created image is used in the model building, training, and deployment portions of the workflow where applicable\.

## MLOps template for model building, training, and deployment with third\-party Git repositories using CodePipeline<a name="sagemaker-projects-templates-git-code-pipeline"></a>
+ **Code repository**: Third\-party Git\. Establish the AWS CodeStar connection from your AWS account to your GitHub user or organization\. Add a tag with the key `sagemaker` and value `true` to this AWS CodeStar connection\.
+ **CI/CD workflow automation**: AWS CodePipeline

This template provides the following resources:
+ Associations with one or more customer\-specified Git repositories\.
+ An AWS CodePipeline pipeline that has source, build, deploy\-to\-staging, and deploy\-to\-production steps\. The source step points to the third\-party Git repository and the build step gets the code from that repository and generates CloudFormation stacks to deploy\. The deploy\-to\-staging and deploy\-to\-production steps deploy the CloudFormation stacks to their respective environments\. There is a manual approval step between the staging and production build steps, so that a MLOps engineer must approve the model before it is deployed to production\.
+ An AWS CodeBuild project to populate the Git repositories with the seed code information\. This requires an AWS CodeStar connection from your AWS account to your account on the Git repository host\.
+ An Amazon S3 bucket to store artifacts, including CodePipeline and CodeBuild artifacts, and any artifacts generated from the SageMaker pipeline runs\.

## MLOps template for model building, training, and deployment with third\-party Git repositories using Jenkins<a name="sagemaker-projects-templates-git-jenkins"></a>

Attach your own Git repository to the project for checking in and managing code versions\. Connect the project to Jenkins to orchestrate your model deployment steps\. Start the model deployment workflow by approving the model registered in the model registry for deployment either manually or automatically\.
+ **Code repository**: Third\-party Git\. Establish the AWS CodeStar connection from your AWS account to your GitHub user or organization\. Add a tag with the key `sagemaker` and value `true` to this AWS CodeStar connection\.
+ **CI/CD workflow automation**: Jenkins

This template provides the following resources:
+ Associations with one or more customer\-specified Git repositories\.
+ Seed code to generate Jenkins pipelines that have source, build, deploy\-to\-staging, and deploy\-to\-production steps\. The source step points to the customer\-specified Git repository\. The build step gets the code from that repository and generates two CloudFormation stacks\. The deploy steps deploy the CloudFormation stacks to their respective environments\. There is an approval step between the staging step and the production step\.
+ An AWS CodeBuild project to populate the Git repositories with the seed code information\. This requires an AWS CodeStar connection from your AWS account to your account on the Git repository host\.
+ An Amazon S3 bucket to store artifacts of the SageMaker project and SageMaker pipeline\.

## Update SageMaker Projects to Use Third\-Party Git Repositories<a name="sagemaker-projects-templates-update"></a>

The managed policy attached to the `AmazonSageMakerServiceCatalogProductsUseRole` role was updated on July 27, 2021 for use with the third\-party Git templates\. Users who onboard to Amazon SageMaker Studio after this date and enable project templates use the new policy\. Users who onboarded prior to this date must update the policy to use these templates\. Use one of the following options to update the policy:
+ Delete role and toggle Studio settings

  1. In the IAM console, delete `AmazonSageMakerServiceCatalogProductsUseRole`\.

  1. In the Studio control panel, choose **Edit Settings**\.

  1. Toggle both settings and then choose **Submit**\.
+ In the IAM console, add the following permissions to `AmazonSageMakerServiceCatalogProductsUseRole`:

  ```
  {
        "Effect": "Allow",
        "Action": [
            "codestar-connections:UseConnection"
        ],
        "Resource": "arn:aws:codestar-connections:*:*:connection/*",
        "Condition": {
            "StringEqualsIgnoreCase": {
                "aws:ResourceTag/sagemaker": "true"
            }
        }
    },
    {
        "Effect": "Allow",
        "Action": [
            "s3:PutObjectAcl"
        ],
        "Resource": [
            "arn:aws:s3:::sagemaker-*"
        ]
    }
  ```