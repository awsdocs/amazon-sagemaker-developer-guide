# SageMaker Autopilot Problem Types<a name="autopilot-automate-model-development-problem-types"></a>

You have the option of focusing the Autopilot experiment on specific problem types, which in turn limits the kind of preprocessing and algorithms that are tried\. 

Your problem type options are as follows: 

**Topics**
+ [Linear regression](#autopilot-automate-model-development-problem-types-linear-regression)
+ [Binary classification](#autopilot-automate-model-development-problem-types-binary-classification)
+ [Multi\-class classification](#autopilot-automate-model-development-problem-types-multi-class-classification)
+ [Automatic problem type detection](#autopilot-automate-model-development-problem-types-api-notes)

 Each of the problem types require a tabular data input with the columns labelled\. In a typical ML environment you set the feature that you are interested in predicting \(objective\) and you can also specify the objective type \(classification or regression\)\. However, with Autopilot these are optional\. It can detect these settings for you\. 

## Linear regression<a name="autopilot-automate-model-development-problem-types-linear-regression"></a>

 One commonly used  linear regression example is home prices prediction\. Provided a dataset of home prices and other features like number of bathrooms and bedrooms, linear regression can create a model for you that takes one or more of these features as an input and then predicts a home price\. When using your own data, it is possible to provide some missing observations \(blank data\), but you are notified when there’s too much missing data for a good predictive model\. 

## Binary classification<a name="autopilot-automate-model-development-problem-types-binary-classification"></a>

 Binary classification models are trained using labeled examples of objects mixed with other examples that are not that object\. For example, there is the Not Hotdog app whose primary function was to evaluate images and tell you if it contains a hotdog or not\. While this provides a humorous look at an AI application, a real\-world example is evaluating the Titanic dataset and making fatality predictions \(alive or dead\)\. Being alive or dead is a binary outcome that can be easily measured\. The provided features such as cabin class, life boat number, or age could all be considered reasonable survivability factors for an ill\-fated transatlantic sea voyage\. 

## Multi\-class classification<a name="autopilot-automate-model-development-problem-types-multi-class-classification"></a>

 Multi\-class refers to the range of possible predictions the model will make\. For example, an emotion detection model might have a handful of possible classes: happy, sad, angry, or surprised\. A model of this type would not only predict each class, it would return the percentage probability of each class\. Another example is found in self\-driving cars where they use models to identify objects like pedestrians, vehicles, stop signs, and green/yellow/red lights\. 

## Automatic problem type detection<a name="autopilot-automate-model-development-problem-types-api-notes"></a>

 When setting a problem type with the AutoML API, you have the option of defining one, or letting Autopilot detect it on your behalf\. When the job is finished, if you set a ProblemType, the ResolvedAttribute’s ProblemType will match the ProblemType you set\. If you left it blank \(or null\), the ProblemType will be whatever Autopilot decides on your behalf\. 