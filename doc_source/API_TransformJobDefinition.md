# TransformJobDefinition<a name="API_TransformJobDefinition"></a>

Defines the input needed to run a transform job using the inference specification specified in the algorithm\.

## Contents<a name="API_TransformJobDefinition_Contents"></a>

 **BatchStrategy**   <a name="SageMaker-Type-TransformJobDefinition-BatchStrategy"></a>
A string that determines the number of records included in a single mini\-batch\.  
 `SingleRecord` means only one record is used per mini\-batch\. `MultiRecord` means a mini\-batch is set to contain as many records that can fit within the `MaxPayloadInMB` limit\.  
Type: String  
Valid Values:` MultiRecord | SingleRecord`   
Required: No

 **Environment**   <a name="SageMaker-Type-TransformJobDefinition-Environment"></a>
The environment variables to set in the Docker container\. We support up to 16 key and values entries in the map\.  
Type: String to string map  
Key Length Constraints: Maximum length of 1024\.  
Key Pattern: `[a-zA-Z_][a-zA-Z0-9_]*`   
Value Length Constraints: Maximum length of 10240\.  
Value Pattern: `[\S\s]*`   
Required: No

 **MaxConcurrentTransforms**   <a name="SageMaker-Type-TransformJobDefinition-MaxConcurrentTransforms"></a>
The maximum number of parallel requests that can be sent to each instance in a transform job\. The default value is 1\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 **MaxPayloadInMB**   <a name="SageMaker-Type-TransformJobDefinition-MaxPayloadInMB"></a>
The maximum payload size allowed, in MB\. A payload is the data portion of a record \(without metadata\)\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 **TransformInput**   <a name="SageMaker-Type-TransformJobDefinition-TransformInput"></a>
A description of the input source and the way the transform job consumes it\.  
Type: [TransformInput](API_TransformInput.md) object  
Required: Yes

 **TransformOutput**   <a name="SageMaker-Type-TransformJobDefinition-TransformOutput"></a>
Identifies the Amazon S3 location where you want Amazon SageMaker to save the results from the transform job\.  
Type: [TransformOutput](API_TransformOutput.md) object  
Required: Yes

 **TransformResources**   <a name="SageMaker-Type-TransformJobDefinition-TransformResources"></a>
Identifies the ML compute instances for the transform job\.  
Type: [TransformResources](API_TransformResources.md) object  
Required: Yes

## See Also<a name="API_TransformJobDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformJobDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformJobDefinition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/TransformJobDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformJobDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformJobDefinition) 