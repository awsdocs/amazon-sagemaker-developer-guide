# LabelingJobForWorkteamSummary<a name="API_LabelingJobForWorkteamSummary"></a>

Provides summary information for a work team\.

## Contents<a name="API_LabelingJobForWorkteamSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-LabelingJobForWorkteamSummary-CreationTime"></a>
The date and time that the labeling job was created\.  
Type: Timestamp  
Required: Yes

 **JobReferenceCode**   <a name="SageMaker-Type-LabelingJobForWorkteamSummary-JobReferenceCode"></a>
A unique identifier for a labeling job\. You can use this to refer to a specific labeling job\.  
Type: String  
Length Constraints: Minimum length of 1\.  
Pattern: `.+`   
Required: Yes

 **LabelCounters**   <a name="SageMaker-Type-LabelingJobForWorkteamSummary-LabelCounters"></a>
Provides information about the progress of a labeling job\.  
Type: [LabelCountersForWorkteam](API_LabelCountersForWorkteam.md) object  
Required: No

 **LabelingJobName**   <a name="SageMaker-Type-LabelingJobForWorkteamSummary-LabelingJobName"></a>
The name of the labeling job that the work team is assigned to\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 **WorkRequesterAccountId**   <a name="SageMaker-Type-LabelingJobForWorkteamSummary-WorkRequesterAccountId"></a>
Type: String  
Pattern: `^\d+$`   
Required: Yes

## See Also<a name="API_LabelingJobForWorkteamSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/LabelingJobForWorkteamSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/LabelingJobForWorkteamSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/LabelingJobForWorkteamSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/LabelingJobForWorkteamSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/LabelingJobForWorkteamSummary) 