# VpcConfig<a name="API_VpcConfig"></a>

Specifies a VPC that your training jobs and hosted models have access to\. Control access to and from your training and model containers by configuring the VPC\. For more information, see [Protect Models by Using an Amazon Virtual Private Cloud](host-vpc.md) and [Protect Training Jobs by Using an Amazon Virtual Private Cloud](train-vpc.md)\.

## Contents<a name="API_VpcConfig_Contents"></a>

 **SecurityGroupIds**   <a name="SageMaker-Type-VpcConfig-SecurityGroupIds"></a>
The VPC security group IDs, in the form sg\-xxxxxxxx\. Specify the security groups for the VPC that is specified in the `Subnets` field\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.  
Required: Yes

 **Subnets**   <a name="SageMaker-Type-VpcConfig-Subnets"></a>
The ID of the subnets in the VPC to which you want to connect your training job or model\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 16 items\.  
Length Constraints: Maximum length of 32\.  
Required: Yes

## See Also<a name="API_VpcConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/VpcConfig) 