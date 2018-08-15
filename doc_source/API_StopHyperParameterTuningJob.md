# StopHyperParameterTuningJob<a name="API_StopHyperParameterTuningJob"></a>

Stops a running hyperparameter tuning job and all running training jobs that the tuning job launched\.

All model artifacts output from the training jobs are stored in Amazon Simple Storage Service \(Amazon S3\)\. All data that the training jobs write to Amazon CloudWatch Logs are still available in CloudWatch\. After the tuning job moves to the `Stopped` state, it releases all reserved resources for the tuning job\.

## Request Syntax<a name="API_StopHyperParameterTuningJob_RequestSyntax"></a>

```
{
   "[HyperParameterTuningJobName](#SageMaker-StopHyperParameterTuningJob-request-HyperParameterTuningJobName)": "string"
}
```

## Request Parameters<a name="API_StopHyperParameterTuningJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [HyperParameterTuningJobName](#API_StopHyperParameterTuningJob_RequestSyntax) **   <a name="SageMaker-StopHyperParameterTuningJob-request-HyperParameterTuningJobName"></a>
The name of the tuning job to stop\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 32\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Elements<a name="API_StopHyperParameterTuningJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_StopHyperParameterTuningJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_StopHyperParameterTuningJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/StopHyperParameterTuningJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/StopHyperParameterTuningJob) 