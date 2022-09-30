# Amazon SageMaker Autopilot datasets and problem types<a name="autopilot-datasets-problem-types"></a>

Amazon SageMaker Autopilot gives you the option in Studio or with the AutoML API of specifying a problem type, such as binary classification or regression, or of detecting it on your behalf based on the data you provide\. Autopilot supports tabular data in which each column contains a feature with a specific data type and each row contains an observation\. 

**Topics**
+ [Autopilot datasets, data types, and formats](#autopilot-datasets)
+ [How to specify training and validation datasets](#autopilot-data-sources-training-or-validation)
+ [How to select features for training](#autopilot-feature-selection)
+ [Amazon SageMaker Autopilot problem types](#autopilot-problem-types)

## Autopilot datasets, data types, and formats<a name="autopilot-datasets"></a>

Autopilot supports tabular data formatted as CSV files or as Parquet files\. For tabular data, each column contains a feature with a specific data type and each row contains an observation\. The properties of these two file formats differ considerably\.
+ **CSV** \(comma\-separated\-values\) is a row\-based file format that stores data in human readable plaintext which a popular choice for data exchange as they are supported by a wide range of applications\.
+ **Parquet** is a column\-based file format where the data is stored and processed more efficiently than row\-based file formats\. This makes them a better option for big data problems\.

The **data types** accepted for columns include numerical, categorical, text, and time series that consists of strings of comma\-separate numbers\. If Autopilot detects it is dealing with** time series** sequences, it processes them through specialized feature transformers provided by the [tsfresh](https://tsfresh.readthedocs.io/en/latest/text/list_of_features.html) library\. This library takes the time series as an input and outputs a feature such as the highest absolute value of the time series or descriptive statistics on autocorrelation\. These outputted features are then used as inputs to one of the three problem types\.

 Autopilot supports building machine learning models on large datasets up to hundreds of GBs\. For details on the default resource limits for input datasets and how to increase them, see [Amazon SageMaker Autopilot quotas](autopilot-quotas.md)

## How to specify training and validation datasets<a name="autopilot-data-sources-training-or-validation"></a>

When using [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html) to create an AutoML job, you must use the `InputDataConfig` parameter to specify the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html) objects that provide input data sources\. Each `AutoMLChannel` has a `ChannelType`, which can be set to either `training` or `validation` values that specify how the data is to be used when building a machine learning model\. At least one data source must be provided and a maximum of two data sources is allowed: one for training data and one for validation data\.

How you split the data into training and validation datasets depends on whether you have one or two data sources\.
+ If you only have **one data source**, the `ChannelType` is set to `training` by default and must have this value\.
  + If the `ValidationFraction` value in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSplitConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSplitConfig.html) is not set, 0\.2 \(20%\) of the data from this source is used for validation by default\. 
  + If the `ValidationFraction` is set to a value between 0 and 1, the dataset is split based on the value specified, where the value specifies the fraction of the dataset used for validation\.
+ If you have **two data sources**, the `ChannelType` of one of the `AutoMLChannel` objects must be set to `training`, the default value\. The `ChannelType` of the other data source must be set to `validation`\. The two data sources must have the same format, either CSV or Parquet, and the same schema\. You must not set the value for the `ValidationFraction` in this case because all of the data from each source is used for either training or validation\. Setting this value will cause an error\. 

## How to select features for training<a name="autopilot-feature-selection"></a>

You can manually select the features to be used in training with the `FeatureSpecificatioS3Uri` attribute of [AutoMLCandidateGenerationConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html) within the [CreateAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html) API with the following format\.

```
{
  "[AutoMLJobConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-AutoMLJobConfig)": {
      "[CandidateGenerationConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html#sagemaker-Type-AutoMLJobConfig-CandidateGenerationConfig)": {
          "[FeatureSpecificiationS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html#sagemaker-Type-AutoMLCandidateGenerationConfig-FeatureSpecificationS3Uri)":"string"
          }
     }
}
```

Selected features should be contained within a JSON file in the following format:

```
{ "FeatureAttributeNames":["col1", "col2", ...] }
```

The values listed in `["col1", "col2", ...]` are case sensitive\. They should be a list of strings containing unique values that are subsets of the column names in the input data\.

**Note**  
The list of columns provided as features cannot include the target column\.

## Amazon SageMaker Autopilot problem types<a name="autopilot-problem-types"></a>

You set the type of problem with the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)` parameter\. This limits the kind of preprocessing and algorithms that Autopilot tries\. After the job is finished, if you had set the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)`, then the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html) will match the `ProblemType` you set\. If you keep it blank \(or `null`\), the `ProblemType` will be whatever Autopilot decides on your behalf\. 

**Note**  
In some cases, Autopilot is unable to infer the `ProblemType` with high enough confidence, in which case you must provide the value for the job to succeed\.

Your problem type options are as follows: 

### Regression<a name="autopilot-automate-model-development-problem-types-regression"></a>

Regression estimates the values of a dependent target variable based on one or more other variables or attributes that are correlated with it\. An example is the prediction of house prices using features like the number of bathrooms and bedrooms, square footage of the house and garden\. Regression analysis can create a model that takes one or more of these features as an input and predicts the price of a house\.

### Binary classification<a name="autopilot-automate-model-development-problem-types-binary-classification"></a>

Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. A medical diagnosis for whether an individual has a disease or not based on the results of diagnostic tests is an example of binary classification\.

### Multiclass classification<a name="autopilot-automate-model-development-problem-types-multiclass-classification"></a>

Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. An example is the prediction of the topic most relevant to a text document\. A document may be classified as being about, say, religion or politics or finance, or about one of several other predefined topic classes\.