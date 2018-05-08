# ResourceConfig<a name="API_ResourceConfig"></a>

Describes the resources, including ML compute instances and ML storage volumes, to use for model training\. 

## Contents<a name="API_ResourceConfig_Contents"></a>

 **InstanceCount**   <a name="SageMaker-Type-ResourceConfig-InstanceCount"></a>
The number of ML compute instances to use\. For distributed training, provide a value greater than 1\.   
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: Yes

 **InstanceType**   <a name="SageMaker-Type-ResourceConfig-InstanceType"></a>
The ML compute instance type\.   
Type: String  
Valid Values:` ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.m5.large | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge`   
Required: Yes

 **VolumeKmsKeyId**   <a name="SageMaker-Type-ResourceConfig-VolumeKmsKeyId"></a>
The Amazon Resource Name \(ARN\) of a AWS Key Management Service key that Amazon SageMaker uses to encrypt data on the storage volume attached to the ML compute instance\(s\) that run the training job\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Required: No

 **VolumeSizeInGB**   <a name="SageMaker-Type-ResourceConfig-VolumeSizeInGB"></a>
The size of the ML storage volume that you want to provision\.   
ML storage volumes store model artifacts and incremental states\. Training algorithms might also use the ML storage volume for scratch space\. If you want to store the training data in the ML storage volume, choose `File` as the `TrainingInputMode` in the algorithm specification\.   
You must specify sufficient ML storage for your scenario\.   
 Amazon SageMaker supports only the General Purpose SSD \(gp2\) ML storage volume type\. 
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: Yes

## See Also<a name="API_ResourceConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ResourceConfig) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ResourceConfig) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ResourceConfig) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ResourceConfig) 