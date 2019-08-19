# InferenceSpecification<a name="API_InferenceSpecification"></a>

Defines how to perform inference generation after a training job is run\.

## Contents<a name="API_InferenceSpecification_Contents"></a>

 **Containers**   <a name="SageMaker-Type-InferenceSpecification-Containers"></a>
The Amazon ECR registry path of the Docker image that contains the inference code\.  
Type: Array of [ModelPackageContainerDefinition](API_ModelPackageContainerDefinition.md) objects  
Array Members: Fixed number of 1 item\.  
Required: Yes

 **SupportedContentTypes**   <a name="SageMaker-Type-InferenceSpecification-SupportedContentTypes"></a>
The supported MIME types for the input data\.  
Type: Array of strings  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: Yes

 **SupportedRealtimeInferenceInstanceTypes**   <a name="SageMaker-Type-InferenceSpecification-SupportedRealtimeInferenceInstanceTypes"></a>
A list of the instance types that are used to generate inferences in real\-time\.  
Type: Array of strings  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.m5.large | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge | ml.c4.large | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge | ml.c5.large | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge | ml.g4dn.xlarge | ml.g4dn.2xlarge | ml.g4dn.4xlarge | ml.g4dn.8xlarge | ml.g4dn.12xlarge | ml.g4dn.16xlarge | ml.r5.large | ml.r5.xlarge | ml.r5.2xlarge | ml.r5.4xlarge | ml.r5.12xlarge | ml.r5.24xlarge`   
Required: Yes

 **SupportedResponseMIMETypes**   <a name="SageMaker-Type-InferenceSpecification-SupportedResponseMIMETypes"></a>
The supported MIME types for the output data\.  
Type: Array of strings  
Length Constraints: Maximum length of 1024\.  
Pattern: `^[-\w]+\/.+$`   
Required: Yes

 **SupportedTransformInstanceTypes**   <a name="SageMaker-Type-InferenceSpecification-SupportedTransformInstanceTypes"></a>
A list of the instance types on which a transformation job can be run or on which an endpoint can be deployed\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\.  
Valid Values:` ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge | ml.m5.large | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge`   
Required: Yes

## See Also<a name="API_InferenceSpecification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/InferenceSpecification) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/InferenceSpecification) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/InferenceSpecification) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/InferenceSpecification) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/InferenceSpecification) 