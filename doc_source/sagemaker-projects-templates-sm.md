# Use SageMaker Provided Project Templates<a name="sagemaker-projects-templates-sm"></a>

SageMaker provides project templates that create the infrastructure you need to create an MLOps solution\. Currently, SageMaker offers the following project templates\.
+ **MLOps template for model building and training** \- This template enables you to build and train machine learning models and register models to the model registry\. Use this template when you want a MLOps solution for building and training models\. This template provides the following resources:
  + An AWS CodeCommit repository that contains sample code that creates SageMaker pipeline in python code and shows how to create and update a SageMaker pipeline\. This repository also has a Python Jupyter notebook that you can open and run in SageMaker Studio\.
  + An CodePipeline that has source and build steps\. The source step points to the CodeCommit repository and the build step gets the code from that repository, creates and/or updates the SageMaker pipeline, starts a pipeline execution, and waits for the pipeline execution to complete\.
  + An Amazon S3 bucket to store artifacts, including CodePipeline and CodeBuild artifacts, and any artifacts generated from the SageMaker pipeline runs\.

  In a collaborative environment with multiple Studio users working on a same project, we recommend creating this project\. After data scientists experiment in SageMaker Studio and check their code in to CodeCommit, model building and training happens in the common infrastructure so that there is a central, authoritative location to keep track of the ML Models and artifacts that are ready to go to production\.
+ **MLOps template for model deployment** \- This template deploys machine learning models from the Amazon SageMaker model registry to SageMaker hosted endpoints for real\-time inference\. Use this template when you have trained models that you want to deploy for inference\. This template provides the following resources:
  + An AWS CodeCommit repository that contains sample code that deploys models to endpoints in staging and production environments\.
  + An CodePipeline that has source, build, deploy to staging, and deploy to production steps\. The source step points to the CodeCommit repository, the build step gets the code from that repository, generates AWS CloudFormation stacks to deploy\. The deploy to staging and deploy to production steps deploy the AWS CloudFormation stacks to their respective environments\. There is a manual approval step between the staging and production build steps, so that a MLOps engineer must approve the model before it is deployed to production\.

    There is also a programmatic approval step with placeholder tests in the example code in the CodeCommit repository\. You can add additional tests to replace the placeholders tests\.
  + An Amazon S3 bucket to store artifacts, including CodePipeline and CodeBuild artifacts, and any artifacts generated from the SageMaker pipeline runs\.

  This template recognizes changes in the model registry\. When a new model version is registered and approved, it automatically triggers a deployment\.
+ **MLOps template for model building, training, and deployment** \- This template enables you to easily build, train, and deploy machine learning models\. Use this template when you want a complete MLOps solution from data preparation to model deployment\.

  This template is a combination of the previous 2 templates, and contains all of the resources provided in those templates\.
