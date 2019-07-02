# StoppingCondition<a name="API_StoppingCondition"></a>

Specifies a limit to how long a model training or compilation job can run\. When the job reaches the time limit, Amazon SageMaker ends the training or compilation job\. Use this API to cap model training costs\.

To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for 120 seconds\. Algorithms can use this 120\-second window to save the model artifacts, so the results of training are not lost\. 

The training algorithms provided by Amazon SageMaker automatically save the intermediate results of a model training job when possible\. This attempt to save artifacts is only a best effort case as model might not be in a state from which it can be saved\. For example, if training has just started, the model might not be ready to save\. When saved, this intermediate data is a valid model artifact\. You can use it to create a model with `CreateModel`\.

**Note**  
The Neural Topic Model \(NTM\) currently does not support saving intermediate model artifacts\. When training NTMs, make sure that the maximum runtime is sufficient for the training job to complete\.

## Contents<a name="API_StoppingCondition_Contents"></a>

 **MaxRuntimeInSeconds**   <a name="SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds"></a>
The maximum length of time, in seconds, that the training or compilation job can run\. If job does not complete during this time, Amazon SageMaker ends the job\. If value is not specified, default value is 1 day\. The maximum value is 28 days\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

## See Also<a name="API_StoppingCondition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/StoppingCondition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/StoppingCondition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/StoppingCondition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/StoppingCondition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/StoppingCondition) 