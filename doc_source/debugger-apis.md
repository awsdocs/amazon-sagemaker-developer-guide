# Amazon SageMaker Debugger API Operations<a name="debugger-apis"></a>

Amazon SageMaker Debugger has API operations in several locations that are used to implement its monitoring and analysis of model training\.

Amazon SageMaker Debugger provides an open source *smdebug* Python library at [awslabs/sagemaker\-debugger](https://github.com/awslabs/sagemaker-debugger/tree/master/smdebug) that is used to configure built\-in rules or to define custom rules used to analyze the tensor data from training jobs\.

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/) is a high\-level SDK focused on machine learning experimentation\. The SDK can be used to deploy built\-in or custom rules defined with the *smdebug* Python library to monitor and analyze these tensors using SageMaker estimators\.

Debugger has added operations and types to the Amazon SageMaker API that enable the platform to use Debugger when training a model and to manage the configuration of inputs and outputs\. 
+  [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) and [ DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) use the following Debugger APIs to configure tensors and rules, hook up the *smdebug* library, and manage the storage of TensorBoard outputs:
  +  [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html)
  +  [ CollectionConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CollectionConfiguration.html)
  +  [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html)
  +  [ TensorBoardOutputConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TensorBoardOutputConfig.html)
+  [ DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) also provides an additional type to report on the status of rule evaluations: 
  +  [ DebugRuleEvaluationStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleEvaluationStatus.html)
+ [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) also has parameters for accessing these Debugger configuration types, in addition to the time and billable time spend in model training\.

Debugger also makes use of the SageMaker Processing functionality when analyzing model training\. For more information on Processing, see [Process Data and Evaluate Models](processing-job.md)\.