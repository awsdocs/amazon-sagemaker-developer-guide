# Amazon SageMaker API Permissions: Actions, Permissions, and Resources Reference<a name="api-permissions-reference"></a>

When you are setting up [Access Control](authentication-and-access-control.md#access-control) and writing a permissions policy that you can attach to an IAM identity \(an identity\-based policy\), use the following table as a reference\. The table lists each Amazon SageMaker API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\. You specify the actions in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field\. 

**Note**  
Except for the `ListTags` API, resource\-level restrictions are not available on `List-` calls \. Any user calling a `List-` API will see all resources of that type in the account\.

To express conditions in your Amazon SageMaker policies, you can use AWS\-wide condition keys\. For a complete list of AWS\-wide keys, see [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**Amazon SageMaker API Operations and Required Permissions for Actions**  

| Amazon SageMaker API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [AddTags](API_AddTags.md)  |  `sagemaker:AddTags`  |  `arn:aws:sagemaker:region:account-id:*`  | 
|  [CreateEndpoint](API_CreateEndpoint.md)  |  `sagemaker:CreateEndpoint` `kms:CreateGrant` \(required only if the associated `EndPointConfig` has a `KmsKeyId` specified\)  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [CreateEndpointConfig](API_CreateEndpointConfig.md)  |  `sagemaker:CreateEndpointConfig`  |  `arn:aws:sagemaker:region:account-id:endpoint-config/endpointConfigName`  | 
|  [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md)  |  `sagemaker:CreateHyperParameterTuningJob` `iam:PassRole`   |  `arn:aws:sagemaker:region:account-id:hyper-parameter-tuning-job/hyperParameterTuningJobName`  | 
|  [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md)  |  `sagemaker:CreatePresignedNotebookInstanceUrl`  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [CreateLabelingJob](API_CreateLabelingJob.md)  |  sagemaker:CreateLabelingJob iam:PassRole  |  `arn:aws:sagemaker:region:account-id:labeling-job/labelingJobName`  | 
|  [CreateModel](API_CreateModel.md)  |  `sagemaker:CreateModel` `iam:PassRole`  |  `arn:aws:sagemaker:region:account-id:model/modelName`  | 
|  [CreateNotebookInstance](API_CreateNotebookInstance.md)  |  `sagemaker:CreateNotebookInstance` `iam:PassRole` `ec2:CreateNetworkInterface` `ec2:AttachNetworkInterface` `ec2:ModifyNetworkInterfaceAttribute` `ec2:DeleteNetworkInterface` `ec2:DescribeAvailabilityZones` `ec2:DescribeInternetGateways` `ec2:DescribeNetworkInterfaces` `ec2:DescribeSecurityGroups` `ec2:DescribeSubnets` `ec2:DescribeVpcs` `kms:CreateGrant` \(required only if you specify a `KmsKeyId`\) `secretsmanager:GetSecretValue` \(required only if you specify an AWS Secrets Manager secret to access a private Git repository\.  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [CreateTrainingJob](API_CreateTrainingJob.md)  |  `sagemaker:CreateTrainingJob` `iam:PassRole` `kms:CreateGrant` \(required only if the associated `ResourceConfig` has a specified `VolumeKmsKeyId` and the associated role does not have a policy that permits this action\)  |  `arn:aws:sagemaker:region:account-id:training-job/trainingJobName`  | 
|  [CreateTransformJob](API_CreateTransformJob.md)  |  `sagemaker:CreateTransformJob` `iam:PassRole` `kms:CreateGrant` \(required only if the associated `TransformResources` has a specified `VolumeKmsKeyId` and the associated role does not have a policy that permits this action\)  |  `arn:aws:sagemaker:region:account-id:transform-job/transformJobName`  | 
|  [CreateWorkteam](API_CreateWorkteam.md)  |  `sagemaker:CreateWorkteam` `cognito-idp:DescribeUserPoolClient` `cognito-idp:UpdateUserPool` `cognito-idp:DescribeUserPool` `cognito-idp:UpdateUserPoolClient`  |  `arn:aws:sagemaker:region:account-id:workteam/private-crowd/work team name` `arn:aws:sagemaker:region:account-id:workteam/vendor-crowd/work team name` `arn:aws:sagemaker:region:account-id:workteam/public-crowd/work team name`  | 
|  [DeleteEndpoint](API_DeleteEndpoint.md)  |  `sagemaker:DeleteEndpoint`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [DeleteEndpointConfig](API_DeleteEndpointConfig.md)  |  `sagemaker:DeleteEndpointConfig`  |  `arn:aws:sagemaker:region:account-id:endpoint-config/endpointConfigName`  | 
|  [DeleteModel](API_DeleteModel.md)  |  `sagemaker:DeleteModel`  |  `arn:aws:sagemaker:region:account-id:model/modelName`  | 
|  [DeleteNotebookInstance](API_DeleteNotebookInstance.md)  |  `sagemaker:DeleteNotebookInstance` `ec2:DeleteNetworkInterface` `ec2:DetachNetworkInterface` `ec2:DescribeAvailabilityZones` `ec2:DescribeInternetGateways` `ec2:DescribeSecurityGroups` `ec2:DescribeSubnets` `ec2:DescribeVpcs`  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [DeleteTags](API_DeleteTags.md)  |  `sagemaker:DeleteTags`  |  `arn:aws:sagemaker:region:account-id:*`  | 
|  [DeleteWorkteam](API_DeleteWorkteam.md)  |  `sagemaker:DeleteWorkteam`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 
|  [DescribeEndpoint](API_DescribeEndpoint.md)  |  `sagemaker:DescribeEndpoint`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [DescribeEndpointConfig](API_DescribeEndpointConfig.md)  |  `sagemaker:DescribeEndpointConfig`  |  `arn:aws:sagemaker:region:account-id:endpoint-config/endpointConfigName`  | 
|  [DescribeHyperParameterTuningJob](API_DescribeHyperParameterTuningJob.md)  |  `sagemaker:DescribeHyperParameterTuningJob`  |  `arn:aws:sagemaker:region:account-id:hyper-parameter-tuning-job/hyperParameterTuningJob`  | 
|  [DescribeLabelingJob](API_DescribeLabelingJob.md)  |  `sagemaker:DescribeLabelingJob`  |  `arn:aws:sagemaker:region:account-id:labeling-job/labelingJobName`  | 
|  [DescribeModel](API_DescribeModel.md)  |  `sagemaker:DescribeModel`  |  `arn:aws:sagemaker:region:account-id:model/modelName`  | 
|  [DescribeNotebookInstance](API_DescribeNotebookInstance.md)  |  `sagemaker:DescribeNotebookInstance `  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [DescribeSubscribedWorkteam](API_DescribeSubscribedWorkteam.md)  |  `sagemaker:DescribeSubscribedWorkteam` `aws-marketplace:ViewSubscriptions`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 
|  [DescribeTrainingJob](API_DescribeTrainingJob.md)  |  `sagemaker:DescribeTrainingJob`  |  `arn:aws:sagemaker:region:account-id:training-job/trainingjobname`  | 
|  [DescribeTransformJob](API_DescribeTransformJob.md)  |  `sagemaker:DescribeTransformJob`  |  `arn:aws:sagemaker:region:account-id:transform-job/transformjobname`  | 
|  [DescribeWorkteam](API_DescribeWorkteam.md)  |  `sagemaker:DescribeWorkteam`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 
|  [InvokeEndpoint](API_runtime_InvokeEndpoint.md)  |  `sagemaker:InvokeEndpoint`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [ListEndpointConfigs](API_ListEndpointConfigs.md)  |  `sagemaker:ListEndpointConfigs`  |  `*`  | 
|  [ListEndpoints](API_ListEndpoints.md)  |  `sagemaker:ListEndpoints`  |  `*`  | 
|  [ListHyperParameterTuningJobs](API_ListHyperParameterTuningJobs.md)  |  `sagemaker:ListHyperParameterTuningJobs`  |  `arn:aws:sagemaker:region:account-id:hyper-parameter-tuning-job/hyperParameterTuningJob`  | 
|  [ListLabelingJobs](API_ListLabelingJobs.md)  |  `sagemaker:ListLabelingJobs`  |  `*`  | 
|  [ListLabelingJobsForWorkteam](API_ListLabelingJobsForWorkteam.md)  |  `sagemaker:ListLabelingJobForWorkteam`  |  `*`  | 
|  [ListModels](API_ListModels.md)  |  `sagemaker:ListModels`  |  `*`  | 
|  [ListNotebookInstances](API_ListNotebookInstances.md)  |  `sagemaker:ListNotebookInstances`  |  `*`  | 
|  [ListSubscribedWorkteams](API_ListSubscribedWorkteams.md)  |  `sagemaker:ListSubscribedWorkteams` `aws-marketplace:ViewSubscriptions`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 
|  [ListTags](API_ListTags.md)  |  `sagemaker:ListTags`  |  `arn:aws:sagemaker:region:account-id:*`  | 
|  [ListTrainingJobs](API_ListTrainingJobs.md)  |  `sagemaker:ListTrainingJobs`  |  `*`  | 
|  [ListTransformJobs](API_ListTransformJobs.md)  |  `sagemaker:ListTransformJobs`  |  `*`  | 
|  [ListTrainingJobsForHyperParameterTuningJob](API_ListTrainingJobsForHyperParameterTuningJob.md)  |  `sagemaker:ListTrainingJobsForHyperParameterTuningJob`  |  `arn:aws:sagemaker:region:account-id:hyper-parameter-tuning-job/hyperParameterTuningJob`  | 
|  [ListWorkteams](API_ListWorkteams.md)  |  `sagemaker:ListWorkteams`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 
|  [StartNotebookInstance](API_StartNotebookInstance.md)  |  `sagemaker:StartNotebookInstance` `iam:PassRole` `ec2:CreateNetworkInterface` `ec2:AttachNetworkInterface` `ec2:ModifyNetworkInterfaceAttribute` `ec2:DescribeAvailabilityZones` `ec2:DescribeInternetGateways` `ec2:DescribeNetworkInterfaces` `ec2:DescribeSecurityGroups` `ec2:DescribeSubnets` `ec2:DescribeVpcs` `kms:CreateGrant` `secretsmanager:GetSecretValue` \(required only if you specify an AWS Secrets Manager secret to access a private Git repository\.  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [StopHyperParameterTuningJob](API_StopHyperParameterTuningJob.md)  |  `sagemaker:StopHyperParameterTuningJob`  |  `arn:aws:sagemaker:region:account-id:hyper-parameter-tuning-job/hyperParameterTuningJob`  | 
|  [StopLabelingJob](API_StopLabelingJob.md)  |  `sagemaker:StopLabelingJob`  |  `arn:aws:sagemaker:region:account-id:labeling-job/labelingJobName`  | 
|  [StopNotebookInstance](API_StopNotebookInstance.md)  |  `sagemaker:StopNotebookInstance`  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [StopTrainingJob](API_StopTrainingJob.md)  |  `sagemaker:StopTrainingJob`  |  `arn:aws:sagemaker:region:account-id:training-job/trainingJobName`  | 
|  [StopTransformJob](API_StopTransformJob.md)  |  `sagemaker:StopTransformJob`  |  `arn:aws:sagemaker:region:account-id:transform-job/transformJobName`  | 
|  [UpdateEndpoint](API_UpdateEndpoint.md)  |  `sagemaker:UpdateEndpoint`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md)  |  `sagemaker:UpdateEndpointWeightsAndCapacities`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [UpdateNotebookInstance](API_UpdateNotebookInstance.md)  |  `sagemaker:UpdateNotebookInstance` `iam:PassRole`  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [UpdateWorkteam](API_UpdateWorkteam.md)  |  `sagemaker:UpdateWorkteam`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 