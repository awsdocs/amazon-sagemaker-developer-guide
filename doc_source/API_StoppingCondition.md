# StoppingCondition<a name="API_StoppingCondition"></a>

Specifies how long model training can run\. When model training reaches the limit, Amazon SageMaker ends the training job\. Use this API to cap model training cost\.

To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for120 seconds\. Algorithms might use this 120\-second window to save the model artifacts, so the results of training is not lost\. 

Training algorithms provided by Amazon SageMaker automatically saves the intermediate results of a model training job \(it is best effort case, as model might not be ready to save as some stages, for example training just started\)\. This intermediate data is a valid model artifact\. You can use it to create a model \(`CreateModel`\)\. 

## Contents<a name="API_StoppingCondition_Contents"></a>

 **MaxRuntimeInSeconds**   <a name="SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds"></a>
The maximum length of time, in seconds, that the training job can run\. If model training does not complete during this time, Amazon SageMaker ends the job\. If value is not specified, default value is 1 day\. Maximum value is 5 days\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

## See Also<a name="API_StoppingCondition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/StoppingCondition) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/StoppingCondition) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/StoppingCondition) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/StoppingCondition) 