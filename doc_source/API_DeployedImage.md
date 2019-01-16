# DeployedImage<a name="API_DeployedImage"></a>

Gets the Amazon EC2 Container Registry path of the docker image of the model that is hosted in this [ProductionVariant](API_ProductionVariant.md)\.

If you used the `registry/repository[:tag]` form to specify the image path of the primary container when you created the model hosted in this `ProductionVariant`, the path resolves to a path of the form `registry/repository[@digest]`\. A digest is a hash value that identifies a specific version of an image\. For information about Amazon ECR paths, see [Pulling an Image](http://docs.aws.amazon.com//AmazonECR/latest/userguide/docker-pull-ecr-image.html) in the *Amazon ECR User Guide*\.

## Contents<a name="API_DeployedImage_Contents"></a>

 **ResolutionTime**   <a name="SageMaker-Type-DeployedImage-ResolutionTime"></a>
The date and time when the image path for the model resolved to the `ResolvedImage`   
Type: Timestamp  
Required: No

 **ResolvedImage**   <a name="SageMaker-Type-DeployedImage-ResolvedImage"></a>
The specific digest path of the image hosted in this `ProductionVariant`\.  
Type: String  
Length Constraints: Maximum length of 255\.  
Pattern: `[\S]+`   
Required: No

 **SpecifiedImage**   <a name="SageMaker-Type-DeployedImage-SpecifiedImage"></a>
The image path you specified when you created the model\.  
Type: String  
Length Constraints: Maximum length of 255\.  
Pattern: `[\S]+`   
Required: No

## See Also<a name="API_DeployedImage_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DeployedImage) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DeployedImage) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DeployedImage) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DeployedImage) 