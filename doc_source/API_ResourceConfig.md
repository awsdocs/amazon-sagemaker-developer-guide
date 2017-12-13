# ResourceConfig<a name="API_ResourceConfig"></a>

Describes the resources, including ML compute instances and ML storage volumes, to use for model training\. 

## Contents<a name="API_ResourceConfig_Contents"></a>

 **InstanceCount**   
The number of ML compute instances to use\. For distributed training, provide a value greater than 1\.   
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: Yes

 **InstanceType**   
The ML compute instance type\.   
Type: String  
Valid Values:` ml.m4.xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.8xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge`   
Required: Yes

 **VolumeSizeInGB**   
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