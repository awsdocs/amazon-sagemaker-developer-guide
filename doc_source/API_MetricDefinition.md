# MetricDefinition<a name="API_MetricDefinition"></a>

Specifies a metric that the training algorithm writes to `stderr` or `stdout`\. Amazon SageMakerHyperparamter tuning captures all defined metrics\. You specify one metric that a hyperparameter tuning job uses as its objective metric to choose the best training job\.

## Contents<a name="API_MetricDefinition_Contents"></a>

 **Name**   <a name="SageMaker-Type-MetricDefinition-Name"></a>
The name of the metric\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Required: Yes

 **Regex**   <a name="SageMaker-Type-MetricDefinition-Regex"></a>
A regular expression that searches the output of a training job and gets the value of the metric\. For more information about using regular expressions to define metrics, see [Defining Objective Metrics](automatic-model-tuning-define-metrics.md)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 500\.  
Required: Yes

## See Also<a name="API_MetricDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/MetricDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/MetricDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/MetricDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/MetricDefinition) 