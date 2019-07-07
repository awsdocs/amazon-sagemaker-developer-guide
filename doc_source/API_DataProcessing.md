# DataProcessing<a name="API_DataProcessing"></a>

The data structure used to specify the data to be used for inference in a batch transform job and to associate the data that is relevant to the prediction results in the output\. The input filter provided allows you to exclude input data that is not needed for inference in a batch transform job\. The output filter provided allows you to include input data relevant to interpreting the predictions in the output from the job\. For more information, see [Associate Prediction Results with their Corresponding Input Records](http://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform-data-processing.html)\.

## Contents<a name="API_DataProcessing_Contents"></a>

 **InputFilter**   <a name="SageMaker-Type-DataProcessing-InputFilter"></a>
A [JSONPath](http://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform-data-processing.html#data-processing-operators) expression used to select a portion of the input data to pass to the algorithm\. Use the `InputFilter` parameter to exclude fields, such as an ID column, from the input\. If you want Amazon SageMaker to pass the entire input dataset to the algorithm, accept the default value `$`\.  
Examples: `"$"`, `"$[1:]"`, `"$.features"`   
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 63\.  
Required: No

 **JoinSource**   <a name="SageMaker-Type-DataProcessing-JoinSource"></a>
Specifies the source of the data to join with the transformed data\. The valid values are `None` and `Input` The default value is `None` which specifies not to join the input with the transformed data\. If you want the batch transform job to join the original input data with the transformed data, set `JoinSource` to `Input`\.   
For JSON or JSONLines objects, such as a JSON array, Amazon SageMaker adds the transformed data to the input JSON object in an attribute called `SageMakerOutput`\. The joined result for JSON must be a key\-value pair object\. If the input is not a key\-value pair object, Amazon SageMaker creates a new JSON file\. In the new JSON file, and the input data is stored under the `SageMakerInput` key and the results are stored in `SageMakerOutput`\.  
For CSV files, Amazon SageMaker combines the transformed data with the input data at the end of the input data and stores it in the output file\. The joined data has the joined input data followed by the transformed data and the output is a CSV file\.   
Type: String  
Valid Values:` Input | None`   
Required: No

 **OutputFilter**   <a name="SageMaker-Type-DataProcessing-OutputFilter"></a>
A [JSONPath](http://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform-data-processing.html#data-processing-operators) expression used to select a portion of the joined dataset to save in the output file for a batch transform job\. If you want Amazon SageMaker to store the entire input dataset in the output file, leave the default value, `$`\. If you specify indexes that aren't within the dimension size of the joined dataset, you get an error\.  
Examples: `"$"`, `"$[0,5:]"`, `"$['id','SageMakerOutput']"`   
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 63\.  
Required: No

## See Also<a name="API_DataProcessing_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DataProcessing) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DataProcessing) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DataProcessing) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DataProcessing) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DataProcessing) 