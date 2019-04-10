# MetricData<a name="API_MetricData"></a>

The name, value, and date and time of a metric that was emitted to Amazon CloudWatch\.

## Contents<a name="API_MetricData_Contents"></a>

 **MetricName**   <a name="SageMaker-Type-MetricData-MetricName"></a>
The name of the metric\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.+`   
Required: No

 **Timestamp**   <a name="SageMaker-Type-MetricData-Timestamp"></a>
The date and time that the algorithm emitted the metric\.  
Type: Timestamp  
Required: No

 **Value**   <a name="SageMaker-Type-MetricData-Value"></a>
The value of the metric\.  
Type: Float  
Required: No

## See Also<a name="API_MetricData_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/MetricData) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/MetricData) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/MetricData) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/MetricData) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/MetricData) 