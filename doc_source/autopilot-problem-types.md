# Amazon SageMaker Autopilot data and problem types<a name="autopilot-problem-types"></a>

Amazon SageMaker Autopilot gives you the option in Studio or with the AutoML API of specifying a problem type, such as binary classification or regression, or of detecting it on your behalf based on the data you provide\. Autopilot supports tabular data in which each column contains a feature with a specific data type and each row contains an observation\. The column data types accepted include numerical, categorical, text, and time series that consists of sequences of comma\-separate numbers\. If Autopilot detects it is dealing with time series sequences, it processes them through specialized feature transformers provided by the [tsfresh](https://tsfresh.readthedocs.io/en/latest/text/list_of_features.html) library that takes the time series as an input and outputs a feature such as the highest absolute value of the time series or descriptive statistics on autocorrelation\. These outputted features are then used as inputs to one of the three problem types\.

You set the type of problem with the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)` parameter\. This limits the kind of preprocessing and algorithms that Autopilot tries\. When the job is finished, if you had set the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)`, then the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html) will match the `ProblemType` you set\. If you leave it blank \(or `null`\), the `ProblemType` will be whatever Autopilot decides on your behalf\. 

**Note**  
In some cases, Autopilot is unable to infer the `ProblemType` with high enough confidence, in which case you must provide the value for the job to succeed\.

Your problem type options are as follows: 

**Topics**
+ [Regression](#autopilot-automate-model-development-problem-types-regression)
+ [Binary classification](#autopilot-automate-model-development-problem-types-binary-classification)
+ [Multiclass classification](#autopilot-automate-model-development-problem-types-multi-class-classification)

## Regression<a name="autopilot-automate-model-development-problem-types-regression"></a>

Regression estimates the values of a dependent target variable based on one or more other variables or attributes that are correlated with it\. An example is the prediction of house prices using features like the number of bathrooms and bedrooms, square footage of the house and garden\. Regression analysis can create a model that takes one or more of these features as an input and predicts the price of a house\.

## Binary classification<a name="autopilot-automate-model-development-problem-types-binary-classification"></a>

Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. A medical diagnosis for whether an individual has a disease or not based on the results of diagnostic tests is an example of binary classification\.

## Multiclass classification<a name="autopilot-automate-model-development-problem-types-multi-class-classification"></a>

Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. An example is the prediction of the topic most relevant to a text document\. A document may be classified as being about, say, religion or politics or finance, or about one of several other predefined topic classes\.