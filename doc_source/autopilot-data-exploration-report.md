# Amazon SageMaker Autopilot Data exploration report<a name="autopilot-data-exploration-report"></a>

Amazon SageMaker Autopilot cleans and pre\-processes your dataset automatically\. High data quality enables more efficient machine learning and produces models that make more accurate predictions\. There are issues with customer\-provided datasets that cannot be fixed automatically without the benefit of some domain knowledge\. Large outlier values in the target column for regression problems, for example, may cause suboptimal predictions for the non\-outlier values\. Outliers may need to be removed depending on the modeling objective\. If a target column is included by accident as one of the input features, the final model will validate well, but be of little value for future predictions\. To help customers discover these sorts of issues, Autopilot provides a data exploration report that contains insights into potential issues with their data and suggests how to handle them\.

A data exploration notebook containing the report is generated for every Autopilot job that completes the pipeline recommendation step\. The report is stored in an S3 bucket and can be accessed from your output path\. The path of the data exploration report usually adheres to the following pattern: 

```
[s3 output path]/[name of the automl job]/sagemaker-automl-candidates/[name of processing job used for data analysis]/notebooks/SageMakerAutopilotDataExplorationNotebook.ipynb
```

The location of the data exploration notebook can be obtained from the Autopilot API using the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html) operation response, stored in [DataExplorationNotebookLocation](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobArtifacts.html#sagemaker-Type-AutoMLJobArtifacts-DataExplorationNotebookLocation)\. 

When running Autopilot from SageMaker Studio, you can open the data exploration report by opening UI that describes the Autopilot job, and then selecting **Open data exploration notebook** from the Autopilot job description page\.

![\[Autopilot job description page with Open data exploration notebook selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-open-data-exploration-notebook.png)

The data exploration report is generated from your data before the training process begins\. This allows you to stop Autopilot jobs that might lead to meaningless results and address any issues or improvements with your dataset before rerunning Autopilot\. This gives you an opportunity to leverage your domain expertise to improve the data quality manually before training a model on a better curated dataset\.

The data report generated contains only static markdown and can be opened in any Jupyter environment\. The notebook that contains the report can be converted to other formats, such as PDF or HTML\. For more information on conversions, see [Using the nbconvert script to convert Jupyter notebooks to other formats\.](https://nbconvert.readthedocs.io/en/latest/usage.html )\.

**Topics**
+ [Dataset Summary](#autopilot-data-exploration-report-dataset-summary)
+ [Target Analysis](#autopilot-data-exploration-report-target-analysis)
+ [Data Sample](#autopilot-data-exploration-report-data-sample)
+ [Duplicate rows](#autopilot-data-exploration-report-duplicate-rows)
+ [Cross column correlations](#autopilot-data-exploration-report-cross-column-correlations)
+ [Anomalous Rows](#autopilot-data-exploration-report-cross-anomolous-rows)
+ [Missing values, cardinality, and descriptive statistics](#autopilot-data-exploration-report-description-statistics-and-values)

## Dataset Summary<a name="autopilot-data-exploration-report-dataset-summary"></a>

This **Dataset Summary** provides key statistics characterizing your dataset\. It is intended to provide you with a quick alert when there are issue with your dataset that Amazon SageMaker Autopilot has detected and that are likely to require your intervention\. The insights are surfaced as warnings that are classified as being of either “high” or “low” severity\. The classification depends on the level of confidence that the issue will adversely impact the performance of the model\.

The high and low severity insights appear in the summary as pop\-ups\. For most of the insights, recommendations are offered for how to confirm that there is an issue with the dataset that requires your attention\. Proposals are also provided for how to resolve the issues\.

Autopilot provides additional statistics about missing or not valid target values in our dataset to help you detect other issues that may not be captured by high severity insights\. An unexpected number of columns of a particular type might indicate that some columns that you want to use may be missing from the dataset\. It could also indicate that there was an issue with how the data was prepared or stored\. Fixing these data problems brought to your attention by Autopilot can improve the performance of the machine learning models trained on your data\. 

![\[Autopilot data report dataset summary.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-dataset-summary.png)

High severity insights are shown in the summary section and in other relevant sections in the report\. Examples of high and low\-severity insights are usually given depending on the section of the data report\.

## Target Analysis<a name="autopilot-data-exploration-report-target-analysis"></a>

Various high and low\-severity insights are shown in this section related to the distribution of values in the target column\. You should check that target column contains the correct values\. Incorrect values in target column will likely result in a machine learning model that doesn't serve the intended business purpose\. Several data insights of high and low severity are present in this section\. Here are several examples\.
+ **Outlier target values **\- Skewed or unusual target distribution for regression, such as heavy tailed targets\.
+ **High or low target cardinality **\- Infrequent number of class labels or a large number of unique classes for classification\.

For both regression and classification problem types, not valid values such as numeric infinity, `NaN` or empty space in target column are surfaced\. Depending on the problem type, different dataset statistics are presented\. A distribution of target column values for a regression problem allows you to verify if the distribution is what you expected\. 

The following example shows an Autopilot data report on the distribution of target column values\.

![\[Autopilot data report on the distribution of target column values.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-target-analysis.png)

Multiple statistics are shown regarding target values and their distribution\. If any of the outliers, not valid values, or missing percentages are greater than zero, these values are surfaced so you can investigate why your data contains unusable target values\. Some unusable target values are highlighted as a low severity insight warning\. In the following example, a ` symbol was added accidentally to the target column, which prevented the numeric value of the target from being parsed\. 

![\[Autopilot data report low severity warning about invalid target values.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-target-analysis-invalid-target-values.png)

To help you identify the problematic values and some impacted rows, Autopilot provides examples of rows that contain unusable or outlier target values\. Distribution of labels for classification are tabulated and plotted so that you can analyze them as well\.

![\[Autopilot data report high cardinality for classification.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-target-analysis-invalid-classification.png)

**Note**  
You can find definitions of all the terms presented in this and other sections in **Definitions** section at the bottom of the report notebook\.

## Data Sample<a name="autopilot-data-exploration-report-data-sample"></a>

To further help you spot issues with your dataset, an actual sample of your data is presented for you to inspect by Amazon SageMaker Autopilot\. The sample table scrolls horizontally\. It can be used to verify that all the necessary columns are present in the dataset used\. If data columns are missing, there may be a preprocessing issue that occurred before importing the dataset that you need to investigate\.

A measure of predictive power is calculated by Amazon SageMaker Autopilot and can be used to identify target columns disguised as input columns\. It helps focus your attention on the columns that might be important because they have high prediction power\. For more information on prediction power, see the **Definitions** section\. 

**Note**  
 It is not recommended that you use prediction power as a substitute for feature importance, unless you're certain that prediction power is an appropriate measure for your use case\.

![\[Autopilot data report data sample prediction power.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-data-sample-prediction.png)

## Duplicate rows<a name="autopilot-data-exploration-report-duplicate-rows"></a>

If duplicate rows are present in the dataset, Amazon SageMaker Autopilot displays a sample of them\.

**Note**  
It is not recommended to balance a dataset by up\-sampling before providing it to Autopilot\. This may result in inaccurate validation scores for the models trained by Autopilot, and the models that are produced may be unusable\.

## Cross column correlations<a name="autopilot-data-exploration-report-cross-column-correlations"></a>

Numeric column correlation is also presented using a standard cross\-correlation matrix graphic\. You can use this to reduce the number of features in the dataset\. A smaller number of features reduces chances of overfitting a model and can reduce the costs of production in two ways\. It lessens the Autopilot runtime needed and, for some applications, can make data collection procedures cheaper\. 

**Note**  
Values close to \+1 and values close to \-1 indicate that two features are highly correlated, positively and negatively, respectively\.

![\[Autopilot data report data cross-correlation matrix.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-data-cross-column-statistics.png)

## Anomalous Rows<a name="autopilot-data-exploration-report-cross-anomolous-rows"></a>

Amazon SageMaker Autopilot detects which rows in your dataset might be anomalous\. It then assigns an anomaly score to each row\. Rows with negative anomaly scores are considered anomalous\.

![\[Autopilot data report data sample prediction power.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-data-anomalous-rows.png)

## Missing values, cardinality, and descriptive statistics<a name="autopilot-data-exploration-report-description-statistics-and-values"></a>

Amazon SageMaker Autopilot examines and reports on properties of the individual columns of your dataset\. In each section of the data report that presents this analysis, the content is arranged in order, so that you can check the most “suspicious” values first\. Using these statistics you can improve contents of individual columns, and improve the quality of the model produced by Autopilot\.

Autopilot calculates several statistics on the categorical values in columns that contain them\. These include the number of unique entries and, for text, the number of unique words\. These are presented in a table for your inspection\.

![\[Autopilot data report statistics on columns with categorical values.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-data-unique-entries-and-words.png)

Autopilot calculates several standard statistics on the numerical values in columns that contain them\. These include the mean, median, minimum and maximum values, and the percentages of numerical types and of outlier values\. These are presented in a table for your inspection\.

![\[Autopilot data report statistics on columns with numerical values.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/autopilot-data-report-data-descriptive-statistics.png)