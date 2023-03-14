# API Reference guide for Amazon SageMaker Autopilot<a name="autopilot-reference"></a>

This section provides a subset of the HTTP service APIs for creating and managing Amazon SageMaker Autopilot resources \(AutoML jobs\) programmatically\.

For information on the entire SageMaker REST APIs and the available SDKs, see [API and SDK Reference](https://docs.aws.amazon.com/sagemaker/latest/dg/api-and-sdk-reference.html)\.

If your language of choice is Python, you can also refer to [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) directly or [AWS SDK for Python \(Boto3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html)\.

## <a name="autopilot-api-reference"></a>

**Actions**

This list details the operations available in the Reference API to manage AutoML jobs programmatically\.
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListAutoMLJobs.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListAutoMLJobs.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopAutoMLJob.html)

**Data Types**

This list details the API AutoML objects used by the actions above as inbound requests or outbound responses\.
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLAlgorithmConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLAlgorithmConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidate.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateStep.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateStep.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLContainerDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLContainerDefinition.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSource.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSource.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSplitConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSplitConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobArtifacts.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobArtifacts.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobCompletionCriteria.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobCompletionCriteria.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobStepMetadata.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobStepMetadata.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobSummary.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobSummary.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLPartialFailureReason.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLPartialFailureReason.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLS3DataSource.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLS3DataSource.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLSecurityConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLSecurityConfig.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CandidateArtifactLocations.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CandidateArtifactLocations.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CandidateProperties.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CandidateProperties.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_FinalAutoMLJobObjectiveMetric.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_FinalAutoMLJobObjectiveMetric.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MetricDatum.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MetricDatum.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelDeployConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelDeployConfig.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelDeployResult.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelDeployResult.html) 
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TuningJobCompletionCriteria.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TuningJobCompletionCriteria.html)