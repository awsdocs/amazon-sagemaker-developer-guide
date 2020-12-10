# Access Management<a name="build-and-manage-access"></a>

The following sections describe the AWS Identity and Access Management \(IAM\) requirements for Amazon SageMaker Model Building Pipelines\. For an example of how you can implement these permissions, see [Prerequisites](define-pipeline.md#define-pipeline-prereq)\.

**Topics**
+ [Pipeline Role Permissions](#build-and-manage-role-permissions)
+ [Pipeline Step Permissions](#build-and-manage-step-permissions)
+ [Service Control Policies with Pipelines](#build-and-manage-scp)

## Pipeline Role Permissions<a name="build-and-manage-role-permissions"></a>

Your pipeline requires an IAM pipeline execution role that is passed to SageMaker Pipelines when you create a pipeline\. The role for the SageMaker instance that is creating the pipeline must have the `iam:PassRole` permission for the pipeline execution role in order to pass it\. For more information on IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)\.

 Your pipeline execution role requires the following permissions: 
+  To pass any role to a SageMaker job within a pipeline, the `iam:PassRole` permission for the role that is being passed\.  
+  `Create` and `Describe` permissions for each of the job types in the pipeline\. 
+  Amazon S3 permissions to use the `JsonGet` function\. You control access to your Amazon S3 resources using resource\-based policies and identity\-based policies\. A resource\-based policy is applied to your Amazon S3 bucket and grants SageMaker Pipelines access to the bucket\. An identity\-based policy gives your pipeline the ability to make Amazon S3 calls from your account\. For more information on resource\-based policies and identity\-based policies, see [Identity\-based policies and resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html)\. 

  ```
  {
          "Action": [
              "s3:GetObject",
              "s3:HeadObject"
          ],
          "Resource": "arn:aws:s3:::<your-bucket-arn>/*",
          "Effect": "Allow"       
     }
  ```

## Pipeline Step Permissions<a name="build-and-manage-step-permissions"></a>

 SageMaker Pipelines include steps that run SageMaker jobs\. In order for the pipeline steps to run these jobs, they require an IAM role in your account that provides access for the needed resource\. This role is passed to the SageMaker service principal by your pipeline\. For more information on IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)\. 

By default, each step takes on the pipeline execution role\. You can optionally pass a different role to any of the steps in your pipeline\. This ensures that the code in each step does not have the ability to impact resources used in other steps unless there is a direct relationship between the two steps specified in the pipeline definition\. You pass these roles when defining the processor or estimator for your step\. For examples of how to include these roles in these definitions, see the [SageMaker Python SDK documentation](https://sagemaker.readthedocs.io/en/stable/overview.html#using-estimators)\. 

## Service Control Policies with Pipelines<a name="build-and-manage-scp"></a>

Service control policies \(SCPs\) are a type of organization policy that you can use to manage permissions in your organization\. SCPs offer central control over the maximum available permissions for all accounts in your organization\. By using SageMaker Pipelines within your organization, you can ensure that data scientists manage your pipeline executions without having to interact with the AWS console\. 

If you’re using a VPC with your SCP that restricts access to Amazon S3, you need to take steps to allow your pipeline to access other Amazon S3 resources\. 

 To allow SageMaker Pipelines to access Amazon S3 outside of your VPC with the `JsonGet` function, update your organization’s SCP to ensure that the role using SageMaker Pipelines can access Amazon S3\. To do this, create an exception for roles that are being used by the SageMaker Pipelines executor via the pipeline execution role using a principal tag and condition key\. 

**To allow SageMaker Pipelines to access Amazon S3 outside of your VPC**

1.  Create a unique tag for your pipeline execution role following the steps in [Tagging IAM users and roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html)\. 

1.  Grant an exception in your SCP using the `Aws:PrincipalTag IAM` condition key for the tag you created\. For more information, see [Creating, updating, and deleting service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_create.html)\. 