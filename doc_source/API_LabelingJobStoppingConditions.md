# LabelingJobStoppingConditions<a name="API_LabelingJobStoppingConditions"></a>

A set of conditions for stopping a labeling job\. If any of the conditions are met, the job is automatically stopped\. You can use these conditions to control the cost of data labeling\.

## Contents<a name="API_LabelingJobStoppingConditions_Contents"></a>

 **MaxHumanLabeledObjectCount**   <a name="SageMaker-Type-LabelingJobStoppingConditions-MaxHumanLabeledObjectCount"></a>
The maximum number of objects that can be labeled by human workers\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 **MaxPercentageOfInputDatasetLabeled**   <a name="SageMaker-Type-LabelingJobStoppingConditions-MaxPercentageOfInputDatasetLabeled"></a>
The maximum number of input data objects that should be labeled\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

## See Also<a name="API_LabelingJobStoppingConditions_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/LabelingJobStoppingConditions) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/LabelingJobStoppingConditions) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/LabelingJobStoppingConditions) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/LabelingJobStoppingConditions) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/LabelingJobStoppingConditions) 