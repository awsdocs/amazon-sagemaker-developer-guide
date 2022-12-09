# Use model cards through the low\-level APIs<a name="model-cards-apis"></a>

You can create an Amazon SageMaker Model Card directly through the SageMaker API or the AWS Command Line Interface \(AWS CLI\)\.

**Note**  
When creating a model card with the low\-level APIs, the content must be in the model card JSON schema and provided as a string\. For more information, see [Model card JSON schema](model-cards.md#model-cards-json-schema)\.

## SageMaker API<a name="model-cards-apis-sagemaker"></a>

Use the following SageMaker API commands to work with Amazon SageMaker Model Cards:
+ [CreateModelCard](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelCard.html)
+ [DescribeModelCard](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeModelCard.html)
+ [ListModelCards](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListModelCards.html)
+ [ListModelCardVersions](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListModelCardVersions.html)
+ [UpdateModelCard](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateModelCard.html)
+ [CreateModelCardExportJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelCardExportJob.html)
+ [DescribeModelCardExportJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeModelCardExportJob.html)
+ [ListModelCardExportJobs](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListModelCardExportJobs.html)
+ [DeleteModelCard](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteModelCard.html)

## AWS CLI<a name="model-cards-apis-cli"></a>

Use the following AWS CLI commands to work with Amazon SageMaker Model Cards:
+ [create\-model\-card](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-model-card.html)
+ [describe\-model\-card](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-model-card.html)
+ [list\-model\-cards](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/list-model-cards.html)
+ [list\-model\-card\-versions](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/list-model-card-versions.html)
+ [update\-model\-card](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/update-model-card.html)
+ [create\-model\-card\-export\-job](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-model-card-export-job.html)
+ [describe\-model\-card\-export\-job](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/describe-model-card-export-job.html)
+ [list\-model\-card\-export\-jobs](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/list-model-card-export-jobs.html)
+ [delete\-model\-card](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/delete-model-card.html)