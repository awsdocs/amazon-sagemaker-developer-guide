# TrainingJobSummary<a name="API_hpo_TrainingJobSummary"></a>

## Contents<a name="API_hpo_TrainingJobSummary_Contents"></a>

 **FinalTuningJobObjectiveMetric**   
Type: [FinalTuningJobObjectiveMetric](API_hpo_FinalTuningJobObjectiveMetric.md) object  
Required: No

 **HyperParameters**   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.  
Required: Yes

 **RetriedAttempt**   
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 **TrainingJobName**   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **TrainingJobStatus**   
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: Yes

## See Also<a name="API_hpo_TrainingJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemakerhpo-2017-11-08/TrainingJobSummary) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemakerhpo-2017-11-08/TrainingJobSummary) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemakerhpo-2017-11-08/TrainingJobSummary) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemakerhpo-2017-11-08/TrainingJobSummary) 