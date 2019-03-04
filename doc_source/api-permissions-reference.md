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
|  [ ListWorkteams Service: Amazon SageMaker Service APIrequestsListWorkteams  Gets a list of work teams that you have defined in a region\. The list may be empty if no work team satisfies the filter specified in the `NameContains` parameter\.  Request Syntax  

```
{
   "[MaxResults](#SageMaker-ListWorkteams-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListWorkteams-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListWorkteams-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListWorkteams-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListWorkteams-request-SortOrder)": "string"
}
```   Request Parameters  For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\. The request accepts the following data in JSON format\.  

 ** [MaxResults](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-MaxResults"></a>
The maximum number of work teams to return in each page of the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No 

 ** [NameContains](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-NameContains"></a>
A string in the work team's name\. This filter returns only work teams whose name contains the specified string\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No 

 ** [NextToken](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-NextToken"></a>
If the result of the previous `ListWorkteams` request was truncated, the response includes a `NextToken`\. To retrieve the next set of labeling jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No 

 ** [SortBy](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-SortBy"></a>
The field to sort results by\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreateDate`   
Required: No 

 ** [SortOrder](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No    Response Syntax  

```
{
   "[NextToken](#SageMaker-ListWorkteams-response-NextToken)": "string",
   "[Workteams](#SageMaker-ListWorkteams-response-Workteams)": [ 
      { 
         "[CreateDate](API_Workteam.md#SageMaker-Type-Workteam-CreateDate)": number,
         "[Description](API_Workteam.md#SageMaker-Type-Workteam-Description)": "string",
         "[LastUpdatedDate](API_Workteam.md#SageMaker-Type-Workteam-LastUpdatedDate)": number,
         "[MemberDefinitions](API_Workteam.md#SageMaker-Type-Workteam-MemberDefinitions)": [ 
            { 
               "[CognitoMemberDefinition](API_MemberDefinition.md#SageMaker-Type-MemberDefinition-CognitoMemberDefinition)": { 
                  "[ClientId](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-ClientId)": "string",
                  "[UserGroup](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserGroup)": "string",
                  "[UserPool](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserPool)": "string"
               }
            }
         ],
         "[ProductListingIds](API_Workteam.md#SageMaker-Type-Workteam-ProductListingIds)": [ "string" ],
         "[SubDomain](API_Workteam.md#SageMaker-Type-Workteam-SubDomain)": "string",
         "[WorkteamArn](API_Workteam.md#SageMaker-Type-Workteam-WorkteamArn)": "string",
         "[WorkteamName](API_Workteam.md#SageMaker-Type-Workteam-WorkteamName)": "string"
      }
   ]
}
```   Response Elements  If the action is successful, the service sends back an HTTP 200 response\. The following data is returned in JSON format by the service\.  

 ** [NextToken](#API_ListWorkteams_ResponseSyntax) **   <a name="SageMaker-ListWorkteams-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of work teams, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\. 

 ** [Workteams](#API_ListWorkteams_ResponseSyntax) **   <a name="SageMaker-ListWorkteams-response-Workteams"></a>
An array of `Workteam` objects, each describing a work team\.  
Type: Array of [Workteam](API_Workteam.md) objects    Errors  For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.   See Also   For more information about using this API in one of the language\-specific AWS SDKs, see the following:    [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListWorkteams)     [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListWorkteams)    ](API_ListWorkteams.md)  |  `sagemaker:ListWorkteams`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 
|  [StartNotebookInstance](API_StartNotebookInstance.md)  |  `sagemaker:StartNotebookInstance` `iam:PassRole` `ec2:CreateNetworkInterface` `ec2:AttachNetworkInterface` `ec2:ModifyNetworkInterfaceAttribute` `ec2:DescribeAvailabilityZones` `ec2:DescribeInternetGateways` `ec2:DescribeSecurityGroups` `ec2:DescribeSubnets` `ec2:DescribeVpcs` `kms:CreateGrant` `secretsmanager:GetSecretValue` \(required only if you specify an AWS Secrets Manager secret to access a private Git repository\.  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [StopHyperParameterTuningJob](API_StopHyperParameterTuningJob.md)  |  `sagemaker:StopHyperParameterTuningJob`  |  `arn:aws:sagemaker:region:account-id:hyper-parameter-tuning-job/hyperParameterTuningJob`  | 
|  [StopLabelingJob](API_StopLabelingJob.md)  |  `sagemaker:StopLabelingJob`  |  `arn:aws:sagemaker:region:account-id:labeling-job/labelingJobName`  | 
|  [StopNotebookInstance](API_StopNotebookInstance.md)  |  `sagemaker:StopNotebookInstance`  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [StopTrainingJob](API_StopTrainingJob.md)  |  `sagemaker:StopTrainingJob`  |  `arn:aws:sagemaker:region:account-id:training-job/trainingJobName`  | 
|  [StopTransformJob](API_StopTransformJob.md)  |  `sagemaker:StopTransformJob`  |  `arn:aws:sagemaker:region:account-id:transform-job/transformJobName`  | 
|  [UpdateEndpoint](API_UpdateEndpoint.md)  |  `sagemaker:UpdateEndpoint`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md)  |  `sagemaker:UpdateEndpointWeightsAndCapacities`  |  `arn:aws:sagemaker:region:account-id:endpoint/endpointName`  | 
|  [UpdateNotebookInstance](API_UpdateNotebookInstance.md)  |  `sagemaker:UpdateNotebookInstance` `iam:PassRole`  |  `arn:aws:sagemaker:region:account-id:notebook-instance/notebookInstanceName`  | 
|  [UpdateWorkteam](API_UpdateWorkteam.md)  |  `sagemaker:UpdateWorkteam`  |  `arn:aws:sagemaker:region:account-id:workteam/*`  | 