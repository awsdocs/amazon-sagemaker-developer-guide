# RenderUiTemplate<a name="API_RenderUiTemplate"></a>

Renders the UI template so that you can preview the worker's experience\. 

## Request Syntax<a name="API_RenderUiTemplate_RequestSyntax"></a>

```
{
   "[RoleArn](#SageMaker-RenderUiTemplate-request-RoleArn)": "string",
   "[Task](#SageMaker-RenderUiTemplate-request-Task)": { 
      "[Input](API_RenderableTask.md#SageMaker-Type-RenderableTask-Input)": "string"
   },
   "[UiTemplate](#SageMaker-RenderUiTemplate-request-UiTemplate)": { 
      "[Content](API_UiTemplate.md#SageMaker-Type-UiTemplate-Content)": "string"
   }
}
```

## Request Parameters<a name="API_RenderUiTemplate_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [RoleArn](#API_RenderUiTemplate_RequestSyntax) **   <a name="SageMaker-RenderUiTemplate-request-RoleArn"></a>
The Amazon Resource Name \(ARN\) that has access to the S3 objects that are used by the template\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** [Task](#API_RenderUiTemplate_RequestSyntax) **   <a name="SageMaker-RenderUiTemplate-request-Task"></a>
A `RenderableTask` object containing a representative task to render\.  
Type: [RenderableTask](API_RenderableTask.md) object  
Required: Yes

 ** [UiTemplate](#API_RenderUiTemplate_RequestSyntax) **   <a name="SageMaker-RenderUiTemplate-request-UiTemplate"></a>
A `Template` object containing the worker UI template to render\.  
Type: [UiTemplate](API_UiTemplate.md) object  
Required: Yes

## Response Syntax<a name="API_RenderUiTemplate_ResponseSyntax"></a>

```
{
   "[Errors](#SageMaker-RenderUiTemplate-response-Errors)": [ 
      { 
         "[Code](API_RenderingError.md#SageMaker-Type-RenderingError-Code)": "string",
         "[Message](API_RenderingError.md#SageMaker-Type-RenderingError-Message)": "string"
      }
   ],
   "[RenderedContent](#SageMaker-RenderUiTemplate-response-RenderedContent)": "string"
}
```

## Response Elements<a name="API_RenderUiTemplate_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Errors](#API_RenderUiTemplate_ResponseSyntax) **   <a name="SageMaker-RenderUiTemplate-response-Errors"></a>
A list of one or more `RenderingError` objects if any were encountered while rendering the template\. If there were no errors, the list is empty\.  
Type: Array of [RenderingError](API_RenderingError.md) objects

 ** [RenderedContent](#API_RenderUiTemplate_ResponseSyntax) **   <a name="SageMaker-RenderUiTemplate-response-RenderedContent"></a>
A Liquid template that renders the HTML for the worker UI\.  
Type: String

## Errors<a name="API_RenderUiTemplate_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_RenderUiTemplate_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/RenderUiTemplate) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/RenderUiTemplate) 