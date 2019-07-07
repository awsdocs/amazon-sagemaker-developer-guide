# HyperParameterTrainingJobDefinition<a name="API_HyperParameterTrainingJobDefinition"></a>

Defines the training jobs launched by a hyperparameter tuning job\.

## Contents<a name="API_HyperParameterTrainingJobDefinition_Contents"></a>

 **AlgorithmSpecification**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-AlgorithmSpecification"></a>
The [HyperParameterAlgorithmSpecification](API_HyperParameterAlgorithmSpecification.md) object that specifies the resource algorithm to use for the training jobs that the tuning job launches\.  
Type: [HyperParameterAlgorithmSpecification](API_HyperParameterAlgorithmSpecification.md) object  
Required: Yes

 **EnableInterContainerTrafficEncryption**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-EnableInterContainerTrafficEncryption"></a>
To encrypt all communications between ML compute instances in distributed training, choose `True`\. Encryption provides greater security for distributed training, but training might take longer\. How long it takes depends on the amount of communication between compute instances, especially if you use a deep learning algorithm in distributed training\.  
Type: Boolean  
Required: No

 **EnableNetworkIsolation**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-EnableNetworkIsolation"></a>
Isolates the training container\. No inbound or outbound network calls can be made, except for calls between peers within a training cluster for distributed training\. If network isolation is used for training jobs that are configured to use a VPC, Amazon SageMaker downloads and uploads customer data and model artifacts through the specified VPC, but the training container does not have network access\.  
The Semantic Segmentation built\-in algorithm does not support network isolation\.
Type: Boolean  
Required: No

 **InputDataConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-InputDataConfig"></a>
An array of [Channel](API_Channel.md) objects that specify the input for the training jobs that the tuning job launches\.  
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
Required: No

 **OutputDataConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-OutputDataConfig"></a>
Specifies the path to the Amazon S3 bucket where you store model artifacts from the training jobs that the tuning job launches\.  
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: Yes

 **ResourceConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-ResourceConfig"></a>
The resources, including the compute instances and storage volumes, to use for the training jobs that the tuning job launches\.  
Storage volumes store model artifacts and incremental states\. Training algorithms might also use storage volumes for scratch space\. If you want Amazon SageMaker to use the storage volume to store the training data, choose `File` as the `TrainingInputMode` in the algorithm specification\. For distributed training algorithms, specify an instance count greater than 1\.  
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes

 **RoleArn**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-RoleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM role associated with the training jobs that the tuning job launches\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 **StaticHyperParameters**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-StaticHyperParameters"></a>
Specifies the values of hyperparameters that do not change for the tuning job\.  
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Key Pattern: `.*`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `.*`   
Required: No

 **StoppingCondition**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-StoppingCondition"></a>
Specifies a limit to how long a model hyperparameter training job can run\. When the job reaches the time limit, Amazon SageMaker ends the training job\. Use this API to cap model training costs\.  
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes

 **VpcConfig**   <a name="SageMaker-Type-HyperParameterTrainingJobDefinition-VpcConfig"></a>
The [VpcConfig](API_VpcConfig.md) object that specifies the VPC that you want the training jobs that this hyperparameter tuning job launches to connect to\. Control access to and from your training container by configuring the VPC\. For more information, see [Protect Training Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html)\.  
Type: [VpcConfig](API_VpcConfig.md) object  
Required: No

## See Also<a name="API_HyperParameterTrainingJobDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTrainingJobDefinition) 