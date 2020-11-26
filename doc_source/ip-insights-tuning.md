# Tune an IP Insights Model<a name="ip-insights-tuning"></a>

*Automatic model tuning*, also called hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric from the metrics that the algorithm computes\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the IP Insights Algorithm<a name="ip-insights-metrics"></a>

The Amazon SageMaker IP Insights algorithm is an unsupervised learning algorithm that learns associations between IP addresses and entities\. The algorithm trains a discriminator model , which learns to separate observed data points \(*positive samples*\) from randomly generated data points \(*negative samples*\)\. Automatic model tuning on IP Insights helps you find the model that can most accurately distinguish between unlabeled validation data and automatically generated negative samples\. The model accuracy on the validation dataset is measured by the area under the receiver operating characteristic curve\. This `validation:discriminator_auc` metric can take values between 0\.0 and 1\.0, where 1\.0 indicates perfect accuracy\.

The IP Insights algorithm computes a `validation:discriminator_auc` metric during validation, the value of which is used as the objective function to optimize for hyperparameter tuning\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| validation:discriminator\_auc |  Area under the receiver operating characteristic curve on the validation dataset\. The validation dataset is not labeled\. Area Under the Curve \(AUC\) is a metric that describes the model's ability to discriminate validation data points from randomly generated data points\.  |  Maximize  | 

## Tunable IP Insights Hyperparameters<a name="ip-insights-tunable-hyperparameters"></a>

You can tune the following hyperparameters for the SageMaker IP Insights algorithm\. 


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| epochs |  IntegerParameterRange  |  MinValue: 1, MaxValue: 100  | 
| learning\_rate |  ContinuousParameterRange  |  MinValue: 1e\-4, MaxValue: 0\.1  | 
| mini\_batch\_size |  IntegerParameterRanges  |  MinValue: 100, MaxValue: 50000  | 
| num\_entity\_vectors |  IntegerParameterRanges  |  MinValue: 10000, MaxValue: 1000000  | 
| num\_ip\_encoder\_layers |  IntegerParameterRanges  |  MinValue: 1, MaxValue: 10  | 
| random\_negative\_sampling\_rate |  IntegerParameterRanges  |  MinValue: 0, MaxValue: 10  | 
| shuffled\_negative\_sampling\_rate |  IntegerParameterRanges  |  MinValue: 0, MaxValue: 10  | 
| vector\_dim |  IntegerParameterRanges  |  MinValue: 8, MaxValue: 256  | 
| weight\_decay |  ContinuousParameterRange  |  MinValue: 0\.0, MaxValue: 1\.0  | 