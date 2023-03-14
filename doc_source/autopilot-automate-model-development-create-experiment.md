# Create an Amazon SageMaker Autopilot experiment<a name="autopilot-automate-model-development-create-experiment"></a>

This guide shows how to create an Amazon SageMaker Autopilot experiment \(that is, how to start an Autopilot job in SageMaker\), so that you can explore, pre\-process, and train various model candidates on a given dataset\. This can help you get started with machine learning quickly\.

You can use a user interface \(Amazon SageMaker Studio UI\) to help you populate the input, output, target, and parameters to run and evaluate an Autopilot experiment or use SageMaker API Reference\. The UI has descriptions, toggle switches, dropdown menus, radio buttons, and more to help you navigate creating your model candidates\. You can also view statistics while the experiment is running\. After it runs, you can compare trials and delve into the details of the pre\-processing steps, algorithms, and hyperparameter ranges of each model\. You also have the option to download their [explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-explainability.html) and [performance](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-insights.html) reports\. Use the provided [ notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development-notebook-output.html ) to see the results of the automated data exploration or the candidate model definitions\.

The following instructions show how to create an Amazon SageMaker Autopilot job as a pilot experiment using Studio UI or SageMaker API reference\. You name your experiment, provide locations for the input and output data, and specify which target data to predict\. Optionally, you can also specify the type of machine learning problem that you want to solve, choose your modeling strategy \(*stacked ensembles* or *hyperparameters optimization*\), select the list of algorithms used by the Autopilot job to train the data, and more\.

## Create an Autopilot experiment using Studio<a name="autopilot-automate-model-development-create-experiment-ui"></a>

**To create an Amazon SageMaker Autopilot experiment using Studio**

1. Sign in at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/), select **Studio** from the left navigation pane, then choose **Open Studio**\.

1. In Studio, choose the home icon \(![\[Home icon in Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\) from the left navigation pane to view the Studio top\-level navigation menu\.

1. On the **Home** tab, choose the **AutoML** card\. This opens a new **AutoML** tab\.

1. Choose **Create an AutoML experiment**\. This opens a new **Create experiment** tab\.

1. In the **Experiment and data details** section, enter the following information:

   1. **Experiment name** – Must be unique to your account in the current AWS Region and contain a maximum of 63 alphanumeric characters\. Can include hyphens \(\-\) but not spaces\.

   1. **Input data** – Provide the Amazon Simple Storage Service \(Amazon S3\) bucket location of your input data\. This S3 bucket must be in your current AWS Region\. The URL must be in an `s3://` format where Amazon SageMaker has write permissions\. The file must be in CSV or Parquet format and contain at least 500 rows\. Select **Browse** to scroll through available paths and **Preview** to see a sample of your input data\.

   1. **Is your S3 input a manifest file?** – A manifest file includes metadata with your input data\. The metadata specifies the location of your data in Amazon S3\. It also specifies how the data is formatted and which attributes from the dataset to use when training your model\. You can use a manifest file as an alternative to preprocessing when your labeled data is being streamed in `Pipe` mode\.

   1. **Auto split data?** – Autopilot can split your data into an 80\-20% split for training and validation data\. If you prefer a custom split, you can choose the **Specify split ratio**\. To use a custom dataset for validation, choose **Provide a validation set**\.

   1. **Output data location \(S3 bucket\)** – The name of the S3 bucket location where you want to store the output data\. The URL for this bucket must be in an Amazon S3 format where Amazon SageMaker has write permissions\. The S3 bucket must be in the current AWS Region\. Autopilot can also create this for you in the same location as your input data\. 

1. Choose **Next: Target and features**\. The **Target and features** tab opens\.

1. In the **Target and features** section, select a column to set as a target for model predictions\. You can also select features for training and change their data type\. The following data types are available: `Text`, `Numerical`, `Categorical`, `Datetime`, `Sequence`, and `Auto`\. All features are selected by default\.

1. Choose **Next: Training method**\. The **Training method** tab opens\.

1. In the **Training method** section, select your training option: **Ensembling**, **Hyperparameter optimization \(HPO\)**, or **Auto** to let Autopilot choose the training method automatically based on the dataset size\. Each training mode runs a pre\-defined set of algorithms on your dataset to train model candidates\. By default, Autopilot pre\-select all the available algorithms for the given training mode\. You can run an Amazon SageMaker Autopilot training experiment with all the algorithms or choose your own subset\.

   For more information on the training modes and the available algorithms, see the **Autopilot training modes** section in the [Training modes and algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html) page\.

1. Choose **Next: Deployment and advanced settings** to open the **Deployment and advanced settings** tab\. Settings include auto display endpoint name, machine learning problem type, and additional choices for running your experiment\.

   1. **Deployment settings** – Autopilot can automatically create an endpoint and deploy your model for you\.

      To auto deploy to an automatically generated endpoint, or to provide an endpoint name for custom deployment, set the toggle to **Yes** under **Auto deploy?** If you are importing data from Amazon SageMaker Data Wrangler, you have additional options to auto deploy the best model with or without the transforms from Data Wrangler\.
**Note**  
If your Data Wrangler flow contains multi\-row operations such as `groupby`, `join`, or `concatenate`, you won't be able to auto deploy with these transforms\. For more information see [Automatically Train Models on Your Data Flow](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-autopilot.html)\.

   1. **Advanced settings \(optional\)** – Autopilot provides additional controls to manually set experimental parameters such as defining your problem type, time constraints on your Autopilot job and trials, security, and encryption settings\.

      1. **Machine learning problem type** – Autopilot can automatically select the machine learning problem type\. If you prefer to choose it manually, use the **Select the machine learning problem type** dropdown menu\.

        1. **Auto** – Autopilot infers the problem type from the values of the attribute that you want to predict\. In some cases, SageMaker is unable to infer accurately\. When that happens, you must provide the value for the job to succeed\.

        1. **Binary classification**– Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes, based on their attributes\. For example, medical diagnosis based on results of diagnostic tests that determine if someone has a disease\.

        1. **Regression** – Regression estimates the values of a dependent target variable based on one or more variables or attributes that are correlated with it\. For example, house prices based on features, such as square footage and number of bathrooms\.

        1. **Multiclass classification** – Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. For example, the prediction of the topic most relevant to a text document, such as politics, finance, or philosophy\.

   1. Choose **Next: Review and create** to get a summary of your Autopilot experiment before you create it\. 

1. Select **Create experiment**\.The creation of the experiment starts an Autopilot job in SageMaker\. Autopilot provides status on the course of the experiment, information on the data exploration process and model candidates in notebooks, a list of generated models and their reports, and the job profile used to create them\.

   For information on the notebooks generated by an Autopilot job, see [Amazon SageMaker Autopilot notebooks generated to manage AutoML tasks](autopilot-automate-model-development-notebook-output.md)\. For information on the details of each model candidate and their reports, see [Models generated by Amazon SageMaker Autopilot ](autopilot-models.md)\.

**Note**  
To avoid incurring unnecessary charges: If you deploy a model that is no longer needed, delete the endpoints and resources that were created during that deployment\. Information about pricing instances by Region is available at [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

## Create an Autopilot experiment programmatically<a name="autopilot-automate-model-development-create-experiment-api"></a>

You can create an AutoMLjob programmatically by calling the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html) API action in any language supported by Amazon SageMaker Autopilot\.

For information on how this API action translates into a function in the language of your choice, see the [ See Also](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#API_CreateAutoMLJob_SeeAlso) section of `CreateAutoMLJob` and choose an SDK\.

As an example, for Python users, see the full request syntax of `[create\_auto\_ml\_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_auto_ml_job)` in AWS SDK for Python\.

### Required parameters<a name="autopilot-create-experiment-api-required-params"></a>

When using [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-AutoMLJobName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-AutoMLJobName) to create an AutoML Job, you must provide the following four values:
+ `AutoMLJobName` to specify the name of your job\.
+  [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html) in `[InputDataConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSource.html)` to specify your data source\.
+ `[OutputDataConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLOutputDataConfig.html)` to specify the Amazon S3 output path to store the artifacts of your AutoML job\.
+ `[RoleArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-RoleArn)` to specify the ARN of the role used to access your data\.

All other parameters are optional\.

### Optional parameters<a name="autopilot-create-experiment-api-optional-params"></a>

The following sections provide details of some additional parameters that you can pass to your AutoML job\.

#### How to set the training mode of an AutoML job<a name="autopilot-set-training-mode"></a>

The set of algorithms run on your data to train your model candidates is dependent on your modeling strategy \(`ENSEMBLING` or `HYPERPARAMETER_TUNING`\)\. You can set the [ training method](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html) of an AutoML job with the `[AutoMLJobConfig\.Mode](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html#sagemaker-Type-AutoMLJobConfig-Mode)` parameter\.

If you keep it blank \(or `null`\), the `Mode` is inferred based on the size of your dataset\.

For information on Autopilot's *stacked ensembles* and *hyperparameters optimization* training methods, see [Training modes and algorithm support](autopilot-model-support-validation.md)

#### How to select features and algorithms for training an AutoML job<a name="autopilot-feature-selection"></a>

##### Features selection<a name="autopilot-automl-job-feature-selection-api"></a>

Autopilot provides automatic data\-preprocessing steps including feature selection and feature extraction\. However, you can manually provide the features to be used in training with the `FeatureSpecificatioS3Uri` attribute of [AutoMLCandidateGenerationConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html) within the [CreateAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html) API with the following format:

```
{
    "[AutoMLJobConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-AutoMLJobConfig)": {
        "[CandidateGenerationConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html#sagemaker-Type-AutoMLJobConfig-CandidateGenerationConfig)": {
            "[FeatureSpecificationS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html#sagemaker-Type-AutoMLCandidateGenerationConfig-FeatureSpecificationS3Uri)":"string"
            },
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

##### Algorithms selection<a name="autopilot-automl-job-algorithms-selection-api"></a>

By default, your Autopilot job runs a pre\-defined list of algorithms on your dataset to train model candidates\. The list of algorithms depends on the training mode \(`ENSEMBLING` or `HYPERPARAMETER_TUNING`\) used by the job\.

You can provide a subset of the default selection of algorithms by adding the `AlgorithmsConfig` attribute and its nested `AutoMLAlgorithms` field to the [AutoMLCandidateGenerationConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html) within the [CreateAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html) API\.

For the list of available algorithms per training `Mode`, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLAlgorithmConfig.html#sagemaker-Type-AutoMLAlgorithmConfig-AutoMLAlgorithms](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLAlgorithmConfig.html#sagemaker-Type-AutoMLAlgorithmConfig-AutoMLAlgorithms)\. For details on each algorithm, see [Training modes and algorithm support](autopilot-model-support-validation.md)\.

The following is an example of a `AlgorithmsConfig` attribute listing exactly three algorithms \("xgboost", "fastai", "catboost"\) in its `AutoMLAlgorithms` field for the ensembling training mode\.

```
{
   "[AutoMLJobConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-AutoMLJobConfig)": {
        "[CandidateGenerationConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobConfig.html#sagemaker-Type-AutoMLJobConfig-CandidateGenerationConfig)": {
            "[AlgorithmsConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidateGenerationConfig.html#sagemaker-Type-AutoMLCandidateGenerationConfig-AlgorithmsConfig)":[
               {"[AutoMLAlgorithms](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLAlgorithmConfig.html#sagemaker-Type-AutoMLAlgorithmConfig-AutoMLAlgorithms)":["xgboost", "fastai", "catboost"]}
            ]
         },
     "Mode": "ENSEMBLING" 
  }
```

#### How to specify the training and validation datasets of an AutoML job<a name="autopilot-data-sources-training-or-validation"></a>

You can provide your own validation dataset and custom data split ratio, or let Autopilot split the dataset automatically\. Each [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLChannel.html) object \(see the required parameter `InputDataConfig`\) has a `ChannelType`, which can be set to either `training` or `validation` values that specify how the data is to be used when building a machine learning model\. At least one data source must be provided and a maximum of two data sources is allowed: one for training data and one for validation data\.

How you split the data into training and validation datasets depends on whether you have one or two data sources\.
+ If you only have **one data source**, the `ChannelType` is set to `training` by default and must have this value\.
  + If the `ValidationFraction` value in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSplitConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLDataSplitConfig.html) is not set, 0\.2 \(20%\) of the data from this source is used for validation by default\. 
  + If the `ValidationFraction` is set to a value between 0 and 1, the dataset is split based on the value specified, where the value specifies the fraction of the dataset used for validation\.
+ If you have **two data sources**, the `ChannelType` of one of the `AutoMLChannel` objects must be set to `training`, the default value\. The `ChannelType` of the other data source must be set to `validation`\. The two data sources must have the same format, either CSV or Parquet, and the same schema\. You must not set the value for the `ValidationFraction` in this case because all of the data from each source is used for either training or validation\. Setting this value causes an error\.

For information on split and cross\-validation in Autopilot see [Cross\-validation in Autopilot](autopilot-metrics-validation.md#autopilot-cross-validation)\.

#### How to set the problem type of an AutoML job<a name="autopilot-set-problem-type-api"></a>

You can set the [ type of problem](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-datasets-problem-types.html#autopilot-problem-types) on an AutoML job with the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)` parameter\. This limits the kind of preprocessing and algorithms that Autopilot tries\. After the job is finished, if you had set the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)`, then the `[ResolvedAttribute\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html)` matches the `ProblemType` you set\. If you keep it blank \(or `null`\), the `ProblemType` is inferred on your behalf\. 

**Note**  
In some cases, Autopilot is unable to infer the `ProblemType` with high enough confidence, in which case you must provide the value for the job to succeed\.