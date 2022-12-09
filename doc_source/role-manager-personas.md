# Persona reference<a name="role-manager-personas"></a>

Amazon SageMaker Role Manager provides suggested permissions for a number of ML personas\. These include user execution roles for common ML practitioner responsibilities as well as service execution roles for common AWS service interactions needed to work with SageMaker\. 

Each persona has suggested permissions in the form of selected ML activities\. For information on predefined ML activities and their permissions, see [ML activity reference](role-manager-ml-activities.md)\. 

## Data scientist persona<a name="role-manager-personas-data-scientist"></a>

Use this persona to configure permissions to perform general machine learning development and experimentation in a SageMaker environment\. This persona includes the following preselected ML activities:
+ Run Studio Applications
+ Manage ML Jobs
+ Manage Models
+ Manage Experiments
+ Search and Visualize Experiments
+ Amazon S3 Bucket Access

## MLOps persona<a name="role-manager-personas-mlops"></a>

Choose this persona to configure permissions for operational activities\. This persona includes the following preselected ML activities:
+ Run Studio Applications
+ Manage Models
+ Manage Endpoints
+ Manage Pipelines
+ Search and Visualize Experiments

## SageMaker compute persona<a name="role-manager-personas-compute"></a>

**Note**  
We recommend that you first use the role manager to create a SageMaker Compute Role so that SageMaker compute resources can perform tasks such as training and inference\. Use the SageMaker Compute Role persona to create this role with the role manager\. After creating a SageMaker Compute Role, take note of its ARN for future use\.

This persona includes the following preselected ML activity:
+ Access Required AWS Services