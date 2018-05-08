# HyperParameterTrainingJobDefinition<a name="API_HyperParameterTrainingJobDefinition"></a>

## Contents<a name="API_HyperParameterTrainingJobDefinition_Contents"></a>

 **AlgorithmSpecification**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-AlgorithmSpecification"></a>
Type: [HyperParameterAlgorithmSpecification](API_HyperParameterAlgorithmSpecification.md) object  
Required: Yes

 **InputDataConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-InputDataConfig"></a>
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: Yes

 **OutputDataConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-OutputDataConfig"></a>
Provides information about how to store model training results \(model artifacts\)\.  
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: Yes

 **ResourceConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-ResourceConfig"></a>
Describes the resources, including ML compute instances and ML storage volumes, to use for model training\.   
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes

 **RoleArn**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-RoleArn"></a>
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 **StaticHyperParameters**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-StaticHyperParameters"></a>
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.  
Required: No

 **StoppingCondition**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-StoppingCondition"></a>
Specifies how long model training can run\. When model training reaches the limit, Amazon SageMaker ends the training job\. Use this API to cap model training cost\.  
To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for120 seconds\. Algorithms might use this 120\-second window to save the model artifacts, so the results of training is not lost\.   
Training algorithms provided by Amazon SageMaker automatically saves the intermediate results of a model training job \(it is best effort case, as model might not be ready to save as some stages, for example training just started\)\. This intermediate data is a valid model artifact\. You can use it to create a model \(`CreateModel`\)\.   
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes

 **VpcConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-VpcConfig"></a>
Specifies a VPC that your training jobs and hosted models have access to\. Control access to and from your training and model containers by configuring the VPC\. For more information, see [Protect Models by Using an Amazon Virtual Private Cloud](host-vpc.md) and [Protect Training Jobs by Using an Amazon Virtual Private Cloud](train-vpc.md)\.  
Type: [VpcConfig](API_VpcConfig.md) object  
Required: No

## See Also<a name="API_HyperParameterTrainingJobDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 