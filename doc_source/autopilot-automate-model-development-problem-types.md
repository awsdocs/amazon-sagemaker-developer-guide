# Amazon SageMaker Autopilot problem types and algorithm support<a name="autopilot-automate-model-development-problem-types"></a>

When setting a problem type, such as binary classification or regression, with the AutoML API, you have the option of specifying it or of letting Amazon SageMaker Autopilot detect it on your behalf\. You set the type of problem with the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)` parameter\. This limits the kind of preprocessing and algorithms that Autopilot tries\. When the job is finished, if you had set the `[CreateAutoPilot\.ProblemType](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAutoMLJob.html#sagemaker-CreateAutoMLJob-request-ProblemType)`, then the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ResolvedAttributes.html) will match the `ProblemType` you set\. If you leave it blank \(or `null`\), the `ProblemType` will be whatever Autopilot decides on your behalf\. 

**Note**  
In some cases, Autopilot is unable to infer the `ProblemType` with high enough confidence, in which case you must provide the value for the job to succeed\.

## Autopilot algorithm support<a name="autopilot-algorithm-suppprt"></a>

Autopilot supports three types of machine learning algorithms to address machine learning problems: 
+  [Linear Learner Algorithm](linear-learner.md): a supervised learning algorithms used for solving either classification or regression problems\.
+ [XGBoost Algorithm](xgboost.md): a supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler and weaker models\.
+ Deep Learning Algorithm: multilayer perceptron \(MLP\), a feedforward artificial neural network that can handle data that is not linear separable\.

**Note**  
You do not need to specify an algorithm to use for your machine learning problem\. Autopilot automatically selects the appropriate algorithm to train\. 

## Autopilot problem types<a name="autopilot-problem-types"></a>

Your problem type options are as follows: 

**Topics**
+ [Regression](#autopilot-automate-model-development-problem-types-regression)
+ [Binary classification](#autopilot-automate-model-development-problem-types-binary-classification)
+ [Multiclass classification](#autopilot-automate-model-development-problem-types-multi-class-classification)

### Regression<a name="autopilot-automate-model-development-problem-types-regression"></a>

Regression estimates the values of a dependent target variable based on one or more other variables or attributes that are correlated with it\. An example is the prediction of house prices using features like the number of bathrooms and bedrooms, square footage of the house and garden\. Regression analysis can create a model that takes one or more of these features as an input and predicts the price of a house\.

### Binary classification<a name="autopilot-automate-model-development-problem-types-binary-classification"></a>

Binary classification is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. A medical diagnosis for whether an individual has a disease or not based on the results of diagnostic tests is an example of binary classification\.

### Multiclass classification<a name="autopilot-automate-model-development-problem-types-multi-class-classification"></a>

Multiclass classification is a type of supervised learning that assigns an individual to one of several classes based on their attributes\. It is supervised because the models are trained using examples where the attributes are provided with correctly labelled objects\. An example is the prediction of the topic most relevant to a text document\. A document may be classified as being about, say, religion or politics or finance, or about one of several other predefined topic classes\.