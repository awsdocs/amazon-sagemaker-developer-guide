# Cross\-Account Support for SageMaker Pipelines<a name="build-and-manage-xaccount"></a>

You can use cross\-account support for Amazon SageMaker Model Building Pipelines to share pipeline entities across AWS accounts and access shared pipelines through direct API calls\. 

## Set up cross\-account pipeline sharing<a name="build-and-manage-xaccount-set-up"></a>

SageMaker uses [AWS Resource Access Manager](https://docs.aws.amazon.com/ram/latest/userguide/what-is.html) \(AWS RAM\) to help you securely share your pipeline entities across accounts\. 

### Create a resource share<a name="build-and-manage-xaccount-set-up-console"></a>

1. Select **Create a resource share** through the [AWS RAM console](https://console.aws.amazon.com/ram/home)\.

1. When specifying resource share details, choose the SageMaker Pipelines resource type and select one or more pipelines that you want to share\. When you share a pipeline with any other account, all of its executions are also shared implicitly\.

1. Associate permissions with your resource share\. Choose either the default read\-only permission policy or the extended pipeline execution permission policy\. For more detailed information, see [Permission policies for SageMaker Pipelines resources](#build-and-manage-xaccount-permissions)\. 
**Note**  
If you select the extended pipeline execution policy, note that any start, stop, and retry commands called by shared accounts use resources in the AWS account that shared the pipeline\.

1. Use AWS account IDs to specify the accounts to which you want to grant access to your shared resources\.

1. Review your resource share configuration and select **Create resource share**\. It may take a few minutes for the resource share and principal associations to complete\.

For more information, see [Sharing your AWS resources](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-sharing.html) in the *AWS Resource Access Manager User Guide*\.

### Get responses to your resource share invitation<a name="build-and-manage-xaccount-set-up-responses"></a>

Once the resource share and principal associations are set, the specified AWS accounts receive an invitation to join the resource share\. The AWS accounts must accept the invite to gain access to any shared resources\.

For more information on accepting a resource share invite through AWS RAM, see [Using shared AWS resources ](https://docs.aws.amazon.com/ram/latest/userguide/getting-started-shared.html)in the *AWS Resource Access Manager User Guide*\.

## Permission policies for SageMaker Pipelines resources<a name="build-and-manage-xaccount-permissions"></a>

When creating your resource share, choose one of two supported permission policies to associate with the SageMaker pipeline resource type\. Both policies grant access to any selected pipeline and all of its executions\. 

### Default read\-only permissions<a name="build-and-manage-xaccount-permissions-default"></a>

The `AWSRAMDefaultPermissionSageMakerPipeline` policy allows the following read\-only actions:

```
"sagemaker:DescribePipeline"
"sagemaker:DescribePipelineDefinitionForExecution"   
"sagemaker:DescribePipelineExecution"
"sagemaker:ListPipelineExecutions"
"sagemaker:ListPipelineExecutionSteps"
"sagemaker:ListPipelineParametersForExecution"
"sagemaker:Search"
```

### Extended pipeline execution permissions<a name="build-and-manage-xaccount-permissions-extended"></a>

The `AWSRAMPermissionSageMakerPipelineAllowExecution` policy includes all of the read\-only permissions from the default policy and also allows shared accounts to start, stop, and retry pipeline executions\.

**Note**  
Be mindful of AWS resource usage when using the extended pipeline execution permission policy\. With this policy, shared accounts are allowed to start, stop, and retry pipeline executions\. Any resources used for shared pipeline executions are consumed by the owner account\. 

The extended pipeline execution permission policy allows the following actions:

```
"sagemaker:DescribePipeline"
"sagemaker:DescribePipelineDefinitionForExecution"   
"sagemaker:DescribePipelineExecution"
"sagemaker:ListPipelineExecutions"
"sagemaker:ListPipelineExecutionSteps"
"sagemaker:ListPipelineParametersForExecution"
"sagemaker:StartPipelineExecution"
"sagemaker:StopPipelineExecution"
"sagemaker:RetryPipelineExecution"
"sagemaker:Search"
```

## Access shared pipeline entities through direct API calls<a name="build-and-manage-xaccount-api-calls"></a>

Once cross\-account pipeline sharing is set up, you can call the following SageMaker API actions using a pipeline ARN:

**Note**  
You can only call API commands if they are included in the permissions associated with your resource share\. If you select the `AWSRAMPermissionSageMakerPipelineAllowExecution` policy, then the start, stop, and retry commands use resources in the AWS account that shared the pipeline\.
+ [DescribePipeline](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribePipeline.html)
+ [DescribePipelineDefinitionForExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribePipelineDefinitionForExecution.html)
+ [DescribePipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribePipelineExecution.html)
+ [ListPipelineExecutions](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListPipelineExecutions.html)
+ [ListPipelineExecutionSteps](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListPipelineExecutionSteps.html)
+ [ListPipelineParametersForExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListPipelineParametersForExecution.html)
+ [StartPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StartPipelineExecution.html)
+ [StopPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopPipelineExecution.html)
+ [RetryPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RetryPipelineExecution.html)