# Amazon SageMaker Debugger API Operations<a name="debugger-apis"></a>

Amazon SageMaker Debugger has API operations in several locations that are used to implement its monitoring and analysis of model training\.

Amazon SageMaker Debugger also provides the open source *SMDebug* Python library at [awslabs/sagemaker\-debugger](https://github.com/awslabs/sagemaker-debugger/tree/master/smdebug) that is used to configure built\-in rules, define custom rules, register hooks to collect output tensor data from training jobs\.

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/) is a high\-level SDK focused on machine learning experimentation\. The SDK can be used to deploy built\-in or custom rules defined with the *SMDebug* Python library to monitor and analyze these tensors using SageMaker estimators\.

Debugger has added operations and types to the Amazon SageMaker API that enable the platform to use Debugger when training a model and to manage the configuration of inputs and outputs\. 
+ [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) and [UpdateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateTrainingJob.html) use the following Debugger APIs to configure tensor collections, rules, rule images, and profiling options:
  + [CollectionConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CollectionConfiguration.html)
  + [DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html)
  + [DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html)
  + [TensorBoardOutputConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TensorBoardOutputConfig.html)
  + [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html)
  + [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html)
+ [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) provides a full description of a training job including the following Debugger configurations and rule evaluation status:
  + [DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html)
  + [DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html)
  + [DebugRuleEvaluationStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleEvaluationStatus.html)
  + [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html)
  + [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html)
  + [ProfilerRuleEvaluationStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleEvaluationStatus.html)

The rule configuration API operations use the SageMaker Processing functionality when analyzing a model training\. For more information about SageMaker Processing, see [Process Data](processing-job.md)\.