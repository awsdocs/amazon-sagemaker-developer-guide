# HumanTaskConfig<a name="API_HumanTaskConfig"></a>

Information required for human workers to complete a labeling task\.

## Contents<a name="API_HumanTaskConfig_Contents"></a>

 **AnnotationConsolidationConfig**   <a name="SageMaker-Type-HumanTaskConfig-AnnotationConsolidationConfig"></a>
Configures how labels are consolidated across human workers\.  
Type: [AnnotationConsolidationConfig](API_AnnotationConsolidationConfig.md) object  
Required: Yes

 **MaxConcurrentTaskCount**   <a name="SageMaker-Type-HumanTaskConfig-MaxConcurrentTaskCount"></a>
Defines the maximum number of data objects that can be labeled by human workers at the same time\. Each object may have more than one worker at one time\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 1000\.  
Required: No

 **NumberOfHumanWorkersPerDataObject**   <a name="SageMaker-Type-HumanTaskConfig-NumberOfHumanWorkersPerDataObject"></a>
The number of human workers that will label an object\.   
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 9\.  
Required: Yes

 **PreHumanTaskLambdaArn**   <a name="SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn"></a>
The Amazon Resource Name \(ARN\) of a Lambda function that is run before a data object is sent to a human worker\. Use this function to provide input to a custom labeling job\.  
For the built\-in bounding box, image classification, semantic segmentation, and text classification task types, Amazon SageMaker Ground Truth provides the following Lambda functions:  
 **US East \(Northern Virginia\) \(us\-east\-1\):**   
+  `arn:aws:lambda:us-east-1:432418664414:function:PRE-BoundingBox` 
+  `arn:aws:lambda:us-east-1:432418664414:function:PRE-ImageMultiClass` 
+  `arn:aws:lambda:us-east-1:432418664414:function:PRE-SemanticSegmentation` 
+  `arn:aws:lambda:us-east-1:432418664414:function:PRE-TextMultiClass` 
 **US East \(Ohio\) \(us\-east\-2\):**   
+  `arn:aws:lambda:us-east-2:266458841044:function:PRE-BoundingBox` 
+  `arn:aws:lambda:us-east-2:266458841044:function:PRE-ImageMultiClass` 
+  `arn:aws:lambda:us-east-2:266458841044:function:PRE-SemanticSegmentation` 
+  `arn:aws:lambda:us-east-2:266458841044:function:PRE-TextMultiClass` 
 **US West \(Oregon\) \(us\-west\-2\):**   
+  `arn:aws:lambda:us-west-2:081040173940:function:PRE-BoundingBox` 
+  `arn:aws:lambda:us-west-2:081040173940:function:PRE-ImageMultiClass` 
+  `arn:aws:lambda:us-west-2:081040173940:function:PRE-SemanticSegmentation` 
+  `arn:aws:lambda:us-west-2:081040173940:function:PRE-TextMultiClass` 
 **EU \(Ireland\) \(eu\-west\-1\):**   
+  `arn:aws:lambda:eu-west-1:568282634449:function:PRE-BoundingBox` 
+  `arn:aws:lambda:eu-west-1:568282634449:function:PRE-ImageMultiClass` 
+  `arn:aws:lambda:eu-west-1:568282634449:function:PRE-SemanticSegmentation` 
+  `arn:aws:lambda:eu-west-1:568282634449:function:PRE-TextMultiClass` 
 **Asia Pacific \(Tokyo \(ap\-northeast\-1\):**   
+  `arn:aws:lambda:ap-northeast-1:477331159723:function:PRE-BoundingBox` 
+  `arn:aws:lambda:ap-northeast-1:477331159723:function:PRE-ImageMultiClass` 
+  `arn:aws:lambda:ap-northeast-1:477331159723:function:PRE-SemanticSegmentation` 
+  `arn:aws:lambda:ap-northeast-1:477331159723:function:PRE-TextMultiClass` 
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:lambda:[a-z]{2}-[a-z]+-\d{1}:\d{12}:function:[a-zA-Z0-9-_\.]+(:(\$LATEST|[a-zA-Z0-9-_]+))?`   
Required: Yes

 **PublicWorkforceTaskPrice**   <a name="SageMaker-Type-HumanTaskConfig-PublicWorkforceTaskPrice"></a>
The price that you pay for each task performed by a public worker\.  
Type: [PublicWorkforceTaskPrice](API_PublicWorkforceTaskPrice.md) object  
Required: No

 **TaskAvailabilityLifetimeInSeconds**   <a name="SageMaker-Type-HumanTaskConfig-TaskAvailabilityLifetimeInSeconds"></a>
The length of time that a task remains available for labelling by human workers\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 864000\.  
Required: No

 **TaskDescription**   <a name="SageMaker-Type-HumanTaskConfig-TaskDescription"></a>
A description of the task for your human workers\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 255\.  
Pattern: `.+`   
Required: Yes

 **TaskKeywords**   <a name="SageMaker-Type-HumanTaskConfig-TaskKeywords"></a>
Keywords used to describe the task so that workers on Amazon Mechanical Turk can discover the task\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 5 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 30\.  
Pattern: `^[A-Za-z0-9]+( [A-Za-z0-9]+)*$`   
Required: No

 **TaskTimeLimitInSeconds**   <a name="SageMaker-Type-HumanTaskConfig-TaskTimeLimitInSeconds"></a>
The amount of time that a worker has to complete a task\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 3600\.  
Required: Yes

 **TaskTitle**   <a name="SageMaker-Type-HumanTaskConfig-TaskTitle"></a>
A title for the task for your human workers\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[\t\n\r -\uD7FF\uE000-\uFFFD]*$`   
Required: Yes

 **UiConfig**   <a name="SageMaker-Type-HumanTaskConfig-UiConfig"></a>
Information about the user interface that workers use to complete the labeling task\.  
Type: [UiConfig](API_UiConfig.md) object  
Required: Yes

 **WorkteamArn**   <a name="SageMaker-Type-HumanTaskConfig-WorkteamArn"></a>
The Amazon Resource Name \(ARN\) of the work team assigned to complete the tasks\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:workteam/.*`   
Required: Yes

## See Also<a name="API_HumanTaskConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HumanTaskConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HumanTaskConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/HumanTaskConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HumanTaskConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HumanTaskConfig) 