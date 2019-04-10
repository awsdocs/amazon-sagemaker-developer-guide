# TrainingJobDefinition<a name="API_TrainingJobDefinition"></a>

Defines the input needed to run a training job using the algorithm\.

## Contents<a name="API_TrainingJobDefinition_Contents"></a>

 **HyperParameters**   <a name="SageMaker-Type-TrainingJobDefinition-HyperParameters"></a>
The hyperparameters used for the training job\.  
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Key Pattern: `.*`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `.*`   
Required: No

 **InputDataConfig**   <a name="SageMaker-Type-TrainingJobDefinition-InputDataConfig"></a>
An array of `Channel` objects, each of which specifies an input source\.  
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: Yes

 **OutputDataConfig**   <a name="SageMaker-Type-TrainingJobDefinition-OutputDataConfig"></a>
the path to the S3 bucket where you want to store model artifacts\. Amazon SageMaker creates subfolders for the artifacts\.  
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: Yes

 **ResourceConfig**   <a name="SageMaker-Type-TrainingJobDefinition-ResourceConfig"></a>
The resources, including the ML compute instances and ML storage volumes, to use for model training\.  
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes

 **StoppingCondition**   <a name="SageMaker-Type-TrainingJobDefinition-StoppingCondition"></a>
Sets a duration for training\. Use this parameter to cap model training costs\.  
To stop a job, Amazon SageMaker sends the algorithm the SIGTERM signal, which delays job termination for 120 seconds\. Algorithms might use this 120\-second window to save the model artifacts\.  
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes

 **TrainingInputMode**   <a name="SageMaker-Type-TrainingJobDefinition-TrainingInputMode"></a>
The input mode used by the algorithm for the training job\. For the input modes that Amazon SageMaker algorithms support, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.  
If an algorithm supports the `File` input mode, Amazon SageMaker downloads the training data from S3 to the provisioned ML storage Volume, and mounts the directory to docker volume for training container\. If an algorithm supports the `Pipe` input mode, Amazon SageMaker streams data directly from S3 to the container\.  
Type: String  
Valid Values:` Pipe | File`   
Required: Yes

## See Also<a name="API_TrainingJobDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TrainingJobDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TrainingJobDefinition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/TrainingJobDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TrainingJobDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TrainingJobDefinition) 