# FinalHyperParameterTuningJobObjectiveMetric<a name="API_FinalHyperParameterTuningJobObjectiveMetric"></a>

Shows the final value for the objective metric for a training job that was launched by a hyperparameter tuning job\. You define the objective metric in the `HyperParameterTuningJobObjective` parameter of [HyperParameterTuningJobConfig](API_HyperParameterTuningJobConfig.md)\.

## Contents<a name="API_FinalHyperParameterTuningJobObjectiveMetric_Contents"></a>

 **MetricName**   <a name="SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-MetricName"></a>
The name of the objective metric\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.+`   
Required: Yes

 **Type**   <a name="SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-Type"></a>
Whether to minimize or maximize the objective metric\. Valid values are Minimize and Maximize\.  
Type: String  
Valid Values:` Maximize | Minimize`   
Required: No

 **Value**   <a name="SageMaker-Type-FinalHyperParameterTuningJobObjectiveMetric-Value"></a>
The value of the objective metric\.  
Type: Float  
Required: Yes

## See Also<a name="API_FinalHyperParameterTuningJobObjectiveMetric_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/FinalHyperParameterTuningJobObjectiveMetric) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/FinalHyperParameterTuningJobObjectiveMetric) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/FinalHyperParameterTuningJobObjectiveMetric) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/FinalHyperParameterTuningJobObjectiveMetric) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/FinalHyperParameterTuningJobObjectiveMetric) 