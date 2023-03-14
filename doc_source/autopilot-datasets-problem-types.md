# Amazon SageMaker Autopilot datasets and problem types<a name="autopilot-datasets-problem-types"></a>

Amazon SageMaker Autopilot gives you the option in Studio or with the AutoML API of specifying a problem type, such as binary classification or regression, or of detecting it on your behalf based on the data you provide\. Autopilot supports tabular data in which each column contains a feature with a specific data type and each row contains an observation\.

**Topics**
+ [Autopilot datasets, data types, and formats](#autopilot-datasets)
+ [Amazon SageMaker Autopilot problem types](#autopilot-problem-types)

## Autopilot datasets, data types, and formats<a name="autopilot-datasets"></a>

Autopilot supports tabular data formatted as CSV files or as Parquet files\. For tabular data, each column contains a feature with a specific data type and each row contains an observation\. The properties of these two file formats differ considerably\.
+ **CSV** \(comma\-separated\-values\) is a row\-based file format that stores data in human readable plaintext which a popular choice for data exchange as they are supported by a wide range of applications\.
+ **Parquet** is a column\-based file format where the data is stored and processed more efficiently than row\-based file formats\. This makes them a better option for big data problems\.

The **data types** accepted for columns include numerical, categorical, text, and time series that consists of strings of comma\-separate numbers\. If Autopilot detects it is dealing with** time series** sequences, it processes them through specialized feature transformers provided by the [tsfresh](https://tsfresh.readthedocs.io/en/latest/text/list_of_features.html) library\. This library takes the time series as an input and outputs a feature such as the highest absolute value of the time series or descriptive statistics on autocorrelation\. These outputted features are then used as inputs to one of the three problem types\.

 Autopilot supports building machine learning models on large datasets up to hundreds of GBs\. For details on the default resource limits for input datasets and how to increase them, see [Amazon SageMaker Autopilot quotas](autopilot-quotas.md)

## Amazon SageMaker Autopilot problem types<a name="autopilot-problem-types"></a>

Your problem type options are as follows: 

### Regression<a name="autopilot-automate-model-development-problem-types-regression"></a>

Regression estimates the values of a dependent target variable based on one or more other variables or attributes that are correlated with it\. An example is the prediction of house prices using features like the number of bathrooms and bedrooms, square footage of the house and garden\. Regression analysis can create a model that takes one or more of these features as an input and predicts the price of a house\.

### Binary classification<a name="autopilot-automate-model-development-problem-types-binary-classification"></a>

Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. A medical diagnosis for whether an individual has a disease or not based on the results of diagnostic tests is an example of binary classification\.

### Multiclass classification<a name="autopilot-automate-model-development-problem-types-multiclass-classification"></a>

Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. An example is the prediction of the topic most relevant to a text document\. A document may be classified as being about, say, religion or politics or finance, or about one of several other predefined topic classes\.