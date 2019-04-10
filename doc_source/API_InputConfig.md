# InputConfig<a name="API_InputConfig"></a>

Contains information about the location of input model artifacts, the name and shape of the expected data inputs, and the framework in which the model was trained\.

## Contents<a name="API_InputConfig_Contents"></a>

 **DataInputConfig**   <a name="SageMaker-Type-InputConfig-DataInputConfig"></a>
Specifies the name and shape of the expected data inputs for your trained model with a JSON dictionary form\. The data inputs are [InputConfig:Framework](#SageMaker-Type-InputConfig-Framework) specific\.   
+  `TensorFlow`: You must specify the name and shape \(NHWC format\) of the expected data inputs using a dictionary format for your trained model\. The dictionary formats required for the console and CLI are different\.
  + Examples for one input:
    + If using the console, `{"input":[1,1024,1024,3]}` 
    + If using the CLI, `{\"input\":[1,1024,1024,3]}` 
  + Examples for two inputs:
    + If using the console, `{"data1": [1,28,28,1], "data2":[1,28,28,1]}` 
    + If using the CLI, `{\"data1\": [1,28,28,1], \"data2\":[1,28,28,1]}` 
+  `MXNET/ONNX`: You must specify the name and shape \(NCHW format\) of the expected data inputs in order using a dictionary format for your trained model\. The dictionary formats required for the console and CLI are different\.
  + Examples for one input:
    + If using the console, `{"data":[1,3,1024,1024]}` 
    + If using the CLI, `{\"data\":[1,3,1024,1024]}` 
  + Examples for two inputs:
    + If using the console, `{"var1": [1,1,28,28], "var2":[1,1,28,28]} ` 
    + If using the CLI, `{\"var1\": [1,1,28,28], \"var2\":[1,1,28,28]}` 
+  `PyTorch`: You can either specify the name and shape \(NCHW format\) of expected data inputs in order using a dictionary format for your trained model or you can specify the shape only using a list format\. The dictionary formats required for the console and CLI are different\. The list formats for the console and CLI are the same\.
  + Examples for one input in dictionary format:
    + If using the console, `{"input0":[1,3,224,224]}` 
    + If using the CLI, `{\"input0\":[1,3,224,224]}` 
  + Example for one input in list format: `[[1,3,224,224]]` 
  + Examples for two inputs in dictionary format:
    + If using the console, `{"input0":[1,3,224,224], "input1":[1,3,224,224]}` 
    + If using the CLI, `{\"input0\":[1,3,224,224], \"input1\":[1,3,224,224]} ` 
  + Example for two inputs in list format: `[[1,3,224,224], [1,3,224,224]]` 
+  `XGBOOST`: input data name and shape are not needed\.
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `[\S\s]+`   
Required: Yes

 **Framework**   <a name="SageMaker-Type-InputConfig-Framework"></a>
Identifies the framework in which the model was trained\. For example: TENSORFLOW\.  
Type: String  
Valid Values:` TENSORFLOW | MXNET | ONNX | PYTORCH | XGBOOST`   
Required: Yes

 **S3Uri**   <a name="SageMaker-Type-InputConfig-S3Uri"></a>
The S3 path where the model artifacts, which result from model training, are stored\. This path must point to a single gzip compressed tar archive \(\.tar\.gz suffix\)\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_InputConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/InputConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/InputConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/InputConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/InputConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/InputConfig) 