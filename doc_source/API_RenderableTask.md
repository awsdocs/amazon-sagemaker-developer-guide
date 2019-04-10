# RenderableTask<a name="API_RenderableTask"></a>

Contains input values for a task\.

## Contents<a name="API_RenderableTask_Contents"></a>

 **Input**   <a name="SageMaker-Type-RenderableTask-Input"></a>
A JSON object that contains values for the variables defined in the template\. It is made available to the template under the substitution variable `task.input`\. For example, if you define a variable `task.input.text` in your template, you can supply the variable in the JSON object as `"text": "sample text"`\.  
Type: String  
Length Constraints: Minimum length of 2\. Maximum length of 128000\.  
Pattern: `[\S\s]+`   
Required: Yes

## See Also<a name="API_RenderableTask_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/RenderableTask) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/RenderableTask) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/RenderableTask) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/RenderableTask) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/RenderableTask) 