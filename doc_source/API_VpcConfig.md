# VpcConfig<a name="API_VpcConfig"></a>

Specifies a VPC that your training jobs and hosted models have access to\. Control access to and from your training and model containers by configuring the VPC\. For more information, see [Protect Endpoints by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/host-vpc.html) and [Protect Training Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html)\. 

## Contents<a name="API_VpcConfig_Contents"></a>

 **SecurityGroupIds**   <a name="SageMaker-Type-VpcConfig-SecurityGroupIds"></a>
The VPC security group IDs, in the form sg\-xxxxxxxx\. Specify the security groups for the VPC that is specified in the `Subnets` field\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.  
Pattern: `[-0-9a-zA-Z]+`   
Required: Yes

 **Subnets**   <a name="SageMaker-Type-VpcConfig-Subnets"></a>
The ID of the subnets in the VPC to which you want to connect your training job or model\.   
Amazon EC2 P3 accelerated computing instances are not available in the c/d/e availability zones of region us\-east\-1\. If you want to create endpoints with P3 instances in VPC mode in region us\-east\-1, create subnets in a/b/f availability zones instead\.
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 16 items\.  
Length Constraints: Maximum length of 32\.  
Pattern: `[-0-9a-zA-Z]+`   
Required: Yes

## See Also<a name="API_VpcConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/VpcConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/VpcConfig) 