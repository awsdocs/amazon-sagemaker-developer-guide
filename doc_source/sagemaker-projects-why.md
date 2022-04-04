# Why Should You Use MLOps?<a name="sagemaker-projects-why"></a>

As you move from running individual artificial intelligence and machine learning \(AI/ML\) projects to using AI/ML to transform your business at scale, the discipline of ML Operations \(MLOps\) can help\. MLOps accounts for the unique aspects of AI/ML projects in project management, CI/CD, and quality assurance, helping you improve delivery time, reduce defects, and make data science more productive\. MLOps refers to a methodology that is built on applying DevOps practices to machine learning workloads\. For a discussion of DevOps principles, see the white paper [Introduction to DevOps on AWS](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/welcome.html?did=wp_card)\. To learn more about implementation using AWS services, see [Practicing CI/CD on AWS](https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf) and [Infrastructure as Code](https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf)\.

Like DevOps, MLOps relies on a collaborative and streamlined approach to the machine learning development lifecycle where the intersection of people, process, and technology optimizes the end\-to\-end activities required to develop, build, and operate machine learning workloads\.

MLOps focuses on the intersection of data science and data engineering in combination with existing DevOps practices to streamline model delivery across the machine learning development lifecycle\. MLOps is the discipline of integrating ML workloads into release management, CI/CD, and operations\. MLOps requires the integration of software development, operations, data engineering, and data science\.

## Challenges with MLOps<a name="sagemaker-projects-why-challenges"></a>

Although MLOps can provide valuable tools to help you scale your business, you might face certain issues as you integrate MLOps into your machine learning workloads\.

**Project management**
+ ML projects involve data scientists, a relatively new role, and one not often integrated into cross\-functional teams\. These new team members often speak a very different technical language than product owners and software engineers, compounding the usual problem of translating business requirements into technical requirements\. 

**Communication and collaboration**
+ Building visibility on ML projects and enabling collaboration across different stakeholders such as data engineers, data scientists, ML engineers, and DevOps is becoming increasingly important to ensure successful outcomes\.



**Everything is code**
+ Use of production data in development activities, longer experimentation lifecycles, dependencies on data pipelines, retraining deployment pipelines, and unique metrics in evaluating the performance of a model\.
+ Models often have a lifecycle independent of the applications and systems integrating with those models\. 
+ The entire end\-to\-end system is reproducible through versioned code and artifacts\. DevOps projects use Infrastructure\-as\-Code \(IaC\) and Configuration\-as\-Code \(CaC\) to build environments, and Pipelines\-as\-Code \(PaC\) to ensure consistent CI/CD patterns\. The pipelines have to integrate with Big Data and ML training workflows\. That often means that the pipeline is a combination of a traditional CI/CD tool and another workflow engine\. There are important policy concerns for many ML projects, so the pipeline may also need to enforce those policies\. Biased input data produces biased results, an increasing concern for business stakeholders\.

**CI/CD**
+ In MLOps, the source data is a first\-class input, along with source code\. That’s why MLOps calls for versioning the source data and initiating pipeline runs when the source or inference data changes\. 
+ Pipelines must also version the ML models, along with inputs and other outputs, in order to provide for traceability\. 
+ Automated testing must include proper validation of the ML model during build phases and when the model is in production\.
+ Build phases may include model training and retraining, a time\-consuming and resource\-intensive process\. Pipelines must be granular enough to only perform a full training cycle when the source data or ML code changes, not when related components change\.
+ Because machine learning code is typically a small part of an overall solution, a deployment pipeline may also incorporate the additional steps required to package a model for consumption as an API by other applications and systems\.

**Monitoring and logging**
+ The feature engineering and model training phases needed to capture model training metrics as well as model experiments\. Tuning an ML model requires manipulating the form of the input data as well as algorithm hyperparameters, and systematically capture those experiments\. Experiment tracking helps data scientists work more effectively and gives a reproducible snapshot of their work\.
+ Deployed ML models require monitoring of the data passed to the model for inference, along with the standard endpoint stability and performance metrics\. The monitoring system must also capture the quality of model output, as evaluated by an appropriate ML metric\. 

## Benefits of MLOps<a name="sagemaker-projects-benefits"></a>

Adopting MLOps practices gives you faster time\-to\-market for ML projects by delivering the following benefits\.
+ **Productivity**: Providing self\-service environments with access to curated data sets lets data engineers and data scientists move faster and waste less time with missing or invalid data\.
+ **Repeatability**: Automating all the steps in the MLDC helps you ensure a repeatable process, including how the model is trained, evaluated, versioned, and deployed\. 
+ **Reliability**: Incorporating CI/CD practices allows for the ability to not only deploy quickly but with increased quality and consistency\. 
+ **Auditability**: Versioning all inputs and outputs, from data science experiments to source data to trained model, means that we can demonstrate exactly how the model was built and where it was deployed\.
+ **Data and model quality**: MLOps lets us enforce policies that guard against model bias and track changes to data statistical properties and model quality over time\. 