# HyperParameterTuningJobObjective<a name="API_HyperParameterTuningJobObjective"></a>

Defines the objective metric for a hyperparameter tuning job\. Hyperparameter tuning uses the value of this metric to evaluate the training jobs it launches, and returns the training job that results in either the highest or lowest value for this metric, depending on the value you specify for the `Type` parameter\.

## Contents<a name="API_HyperParameterTuningJobObjective_Contents"></a>

 **MetricName**   <a name="SageMaker-Type-HyperParameterTuningJobObjective-MetricName"></a>
The name of the metric to use for the objective metric\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Required: Yes

 **Type**   <a name="SageMaker-Type-HyperParameterTuningJobObjective-Type"></a>
Whether to minimize or maximize the objective metric\.  
Type: String  
Valid Values:` Maximize | Minimize`   
Required: Yes

## See Also<a name="API_HyperParameterTuningJobObjective_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTuningJobObjective) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTuningJobObjective) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTuningJobObjective) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTuningJobObjective) 