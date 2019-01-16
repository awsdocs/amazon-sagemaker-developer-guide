# HyperParameterTuningJobWarmStartConfig<a name="API_HyperParameterTuningJobWarmStartConfig"></a>

Specifies the configuration for a hyperparameter tuning job that uses one or more previous hyperparameter tuning jobs as a starting point\. The results of previous tuning jobs are used to inform which combinations of hyperparameters to search over in the new tuning job\.

All training jobs launched by the new hyperparameter tuning job are evaluated by using the objective metric, and the training job that performs the best is compared to the best training jobs from the parent tuning jobs\. From these, the training job that performs the best as measured by the objective metric is returned as the overall best training job\.

**Note**  
All training jobs launched by parent hyperparameter tuning jobs and the new hyperparameter tuning jobs count against the limit of training jobs for the tuning job\.

## Contents<a name="API_HyperParameterTuningJobWarmStartConfig_Contents"></a>

 **ParentHyperParameterTuningJobs**   <a name="SageMaker-Type-HyperParameterTuningJobWarmStartConfig-ParentHyperParameterTuningJobs"></a>
An array of hyperparameter tuning jobs that are used as the starting point for the new hyperparameter tuning job\. For more information about warm starting a hyperparameter tuning job, see [Using a Previous Hyperparameter Tuning Job as a Starting Point](http://docs.aws.amazon.com/automatic-model-tuning-incremental)\.  
Hyperparameter tuning jobs created before October 1, 2018 cannot be used as parent jobs for warm start tuning jobs\.  
Type: Array of [ParentHyperParameterTuningJob](API_ParentHyperParameterTuningJob.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 5 items\.  
Required: Yes

 **WarmStartType**   <a name="SageMaker-Type-HyperParameterTuningJobWarmStartConfig-WarmStartType"></a>
Specifies one of the following:    
IDENTICAL\_DATA\_AND\_ALGORITHM  
The new hyperparameter tuning job uses the same input data and training image as the parent tuning jobs\. You can change the hyperparameter ranges to search and the maximum number of training jobs that the hyperparameter tuning job launches\. You cannot use a new version of the training algorithm, unless the changes in the new version do not affect the algorithm itself\. For example, changes that improve logging or adding support for a different data format are allowed\. You can also change hyperparameters from tunable to static, and from static to tunable, but the total number of static plus tunable hyperparameters must remain the same as it is in all parent jobs\. The objective metric for the new tuning job must be the same as for all parent jobs\.  
TRANSFER\_LEARNING  
The new hyperparameter tuning job can include input data, hyperparameter ranges, maximum number of concurrent training jobs, and maximum number of training jobs that are different than those of its parent hyperparameter tuning jobs\. The training image can also be a different version from the version used in the parent hyperparameter tuning job\. You can also change hyperparameters from tunable to static, and from static to tunable, but the total number of static plus tunable hyperparameters must remain the same as it is in all parent jobs\. The objective metric for the new tuning job must be the same as for all parent jobs\.
Type: String  
Valid Values:` IdenticalDataAndAlgorithm | TransferLearning`   
Required: Yes

## See Also<a name="API_HyperParameterTuningJobWarmStartConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterTuningJobWarmStartConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterTuningJobWarmStartConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterTuningJobWarmStartConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterTuningJobWarmStartConfig) 