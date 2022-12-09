# ML activity reference<a name="role-manager-ml-activities"></a>

ML activities are common AWS tasks related to machine learning with SageMaker that require specific IAM permissions\. Each [persona](https://docs.aws.amazon.com/sagemaker/latest/dg/role-manager-personas.html) suggests related ML activities when creating a role with Amazon SageMaker Role Manager\. You can select any additional ML activities or deselect any suggested ML activities to create a role that meets your unique business needs\.

Amazon SageMaker Role Manager provides predefined permissions for the following ML activities:


****  

| **ML activity** | **Description** | 
| --- | --- | 
| Access Required AWS Services | Permissions to access Amazon S3, Amazon ECR, Amazon CloudWatch, and Amazon EC2\. Required for execution roles for jobs and endpoints\. | 
| Run Studio Applications | Permissions to operate within a Studio environment\. Required for domain and user profile execution roles\. | 
| Manage ML Jobs | Permissions to audit, query lineage, and visualize experiments\. | 
| Manage Models | Permissions to manage SageMaker jobs across their lifecycles\. | 
| Manage Endpoints | Permissions to manage SageMaker endpoint deployments and updates\. | 
| Manage Pipelines | Permissions to manage SageMaker pipelines and pipeline executions\. | 
| Manage Experiments | Permissions to manage SageMaker experiments and trials\. | 
| Search and Visualize Experiments | Permissions to audit, query lineage, and visualize experiments\. | 
| Manage Model Monitoring | Permissions to manage monitoring schedules for SageMaker Model Monitor\. | 
| S3 Full Access | Permissions to perform all Amazon S3 operations\. | 
| S3 Bucket Access | Permissions to perform operations on specified S3 buckets\. | 
| Query Athena Workgroups | Permissions to run and manage Amazon Athena queries\. | 