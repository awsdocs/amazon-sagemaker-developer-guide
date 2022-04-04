# What is a SageMaker Project?<a name="sagemaker-projects-whatis"></a>

By using a SageMaker project, teams of data scientists and developers can work on machine learning business problems by creating a SageMaker project with a SageMaker\-provided MLOps template that automates the model building and deployment pipelines using continuous integrations and continuous delivery \(CI/CD\)\. A SageMaker\-provided template provisions the initial setup required for a complete end\-to\-end MLOps system including model building, training, and deployment, depending on the template you choose\. You can also provide your own custom template to provision the resources you need for your MLOps system\.

A SageMaker project is an AWS Service Catalog–provisioned product with which you can create an end\-to\-end ML solution\. For information about AWS Service Catalog, see [What is AWS Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html)\.

Each SageMaker project has a unique name and ID that are passed to all SageMaker and AWS resources created in the project\. By using the name and ID, you can view all entities associated with your project\. These include:
+ Pipelines
+ Registered models
+ Deployed models \(endpoints\)
+ Datasets
+ AWS Service Catalog products
+ AWS CodePipeline pipelines and Jenkins pipelines
+ AWS CodeCommit repositories and third\-party Git repositories

A typical SageMaker project with a SageMaker\-provided template might include the following\.
+ One or more repositories with sample code for building and deploying ML solutions\. These are working examples that you can clone locally to explore the code provided by SageMaker and modify it for your needs\. You own this code and you can use the repositories as version control for your work\.
+ A SageMaker pipeline that defines steps for data preparation, training, model evaluation, and model deployment\.
+ A CodePipeline pipeline or Jenkins pipeline that runs your SageMaker pipeline every time you check in a new version of the code\. For information about CodePipeline, see [What is AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)\. For information about Jenkins, see [Jenkins User Documentation](https://www.jenkins.io/doc/)\.
+ A model group that contains model versions\. Each time the SageMaker pipeline runs, and the resulting model version is accepted in the conditional validation step, a new model version is deployed to a SageMaker endpoint\. 

## When Should You Use a SageMaker Project?<a name="sagemaker-projects-when"></a>

While notebooks are helpful for model building and experimentation, when you have a team of data scientists and ML engineers working on an ML problem, you need a more scalable way to maintain code consistency and have stricter version control\. Having the code only in notebook files makes it harder to collaborate and risks losing code or model artifacts if the notebook is accidentally deleted or changed\. By using SageMaker projects, you can manage the versions for your Git repositories so you can collaborate across teams more efficiently, ensure code consistency, and implement CI/CD\. 

In addition to managing code, SageMaker projects enable MLOps for model building, model deployment, and end\-to\-end ML workflows\. You can run training jobs or SageMaker pipelines to build models in Amazon SageMaker Studio\. However, if you want to create a CI/CD system that generates models based on events, such as when someone checks in a code change, then consider creating a SageMaker project and using a SageMaker\-provided template\. For a list of the project templates that SageMaker provides, see [Use SageMaker\-Provided Project Templates](sagemaker-projects-templates-sm.md)\.

## Do I Need to Create a Project to Use SageMaker Pipelines?<a name="sagemaker-projects-need"></a>

No\. SageMaker pipelines are standalone entities just like training jobs, processing jobs, and other SageMaker jobs within SageMaker\. You can create, update, and run pipelines directly within a notebook by using the SageMaker Python SDK without using a SageMaker project\.

Projects provide an additional layer to help you organize your code and adopt operational best practices that you need for a production\-quality system\.