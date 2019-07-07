# HyperParameterTuningJobConfig<a name="API_HyperParameterTuningJobConfig"></a>

Configures a hyperparameter tuning job\.

## Contents<a name="API_HyperParameterTuningJobConfig_Contents"></a>

 **HyperParameterTuningJobObjective**   <a name="SageMaker-Type-HyperParameterTuningJobConfig-HyperParameterTuningJobObjective"></a>
The [HyperParameterTuningJobObjective](API_HyperParameterTuningJobObjective.md) object that specifies the objective metric for this tuning job\.  
Type: [HyperParameterTuningJobObjective](API_HyperParameterTuningJobObjective.md) object  
Required: No

 **ParameterRanges**   <a name="SageMaker-Type-HyperParameterTuningJobConfig-ParameterRanges"></a>
The [ParameterRanges](API_ParameterRanges.md) object that specifies the ranges of hyperparameters that this tuning job searches\.  
Type: [ParameterRanges](API_ParameterRanges.md) object  
Required: No

 **ResourceLimits**   <a name="SageMaker-Type-HyperParameterTuningJobConfig-ResourceLimits"></a>
The [ResourceLimits](API_ResourceLimits.md) object that specifies the maximum number of training jobs and parallel training jobs for this tuning job\.  
Type: [ResourceLimits](API_ResourceLimits.md) object  
Required: Yes

 **Strategy**   <a name="SageMaker-Type-HyperParameterTuningJobConfig-Strategy"></a>
Specifies how hyperparameter tuning chooses the combinations of hyperparameter values to use for the training job it launches\. To use the Bayesian search stategy, set this to `Bayesian`\. To randomly search, set it to `Random`\. For information about search strategies, see [How Hyperparameter Tuning Works](http://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-how-it-works.html)\.  
Type: String  
Valid Values:` Bayesian | Random`   
Required: Yes

 **TrainingJobEarlyStoppingType**   <a name="SageMaker-Type-HyperParameterTuningJobConfig-TrainingJobEarlyStoppingType"></a>
Specifies whether to use early stopping for training jobs launched by the hyperparameter tuning job\. This can be one of the following values \(the default value is `OFF`\):    
OFF  
Training jobs launched by the hyperparameter tuning job do not use early stopping\.  
AUTO  
Amazon SageMaker stops training jobs launched by the hyperparameter tuning job when they are unlikely to perform better than previously completed training jobs\. For more information, see [Stop Training Jobs Early](http://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-early-stopping.html)\.
Type: String  
Valid Values:` Off | Auto`   
Required: No

## See Also<a name="API_HyperParameterTuningJobConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTuningJobConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTuningJobConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/HyperParameterTuningJobConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTuningJobConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTuningJobConfig) 