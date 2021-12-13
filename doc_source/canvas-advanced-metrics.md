# Use advanced metrics in your analyses<a name="canvas-advanced-metrics"></a>

Amazon SageMaker Canvas uses different advanced performance metrics to give you a sense of how well your model performed\. The advanced metrics that SageMaker Canvas shows you depend on whether your model performs a numeric or categorical prediction on your data\.

*Numeric prediction* refers to the mathematical concept of regression\. When your **Target column** has values that can be measured, such as yearly revenue or the number of items sold by a department store, Amazon SageMaker Canvas builds a model on your data using regression\. For more information about regression, see [Metrics for numeric prediction](#canvas-numeric-prediction-metrics)\.

*Categorical prediction*, such as 2 category prediction or 3 category prediction, refers to the mathematical concept of classification\. Categorical prediction can be performed on data that can be put into a category:
+ The colors on a color wheel
+ Instances where the data is either a 0 or 1
+ Instances where the data is either a Yes or a No\.
+ A list of responses to a survey question\.

SageMaker Canvas uses confusion matrices to help you visualize when a model makes predictions correctly\.

The following image is an example of a confusion matrix for 2 categories\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-binary-confusion-matrix.png)

The following image is an example of a confusion matrix for 3\+ categories\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-multiclass-confusion-matrix.png)

## Metrics for numeric prediction<a name="canvas-numeric-prediction-metrics"></a>

The following defines the advanced metrics for numeric prediction in Amazon SageMaker Canvas and gives you information about how you can use them\.
+ R2 – The percentage of the difference in the target column that can be explained by the input column\.
+ MAE – Mean absolute error\. On average, the prediction for the target column is \+/\- \{MAE\} from the actual value\.
+ MAPE – Mean absolute percent error\. On average, the prediction for the target column is \+/\- \{MAPE\} % from the actual value
+ RMSE – Root Mean Square Error\. The standard deviation of the errors\.

The following image shows a graph of the residuals or errors\. The horizontal line indicates an error of 0 or a perfect prediction\. The blue dots are the errors\. Their distance from the horizontal line indicates the magnitude of the errors\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-advanced-metrics-residuals.png)

The following image shows an error density plot\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-advanced-metrics-error-density.png)

## Metrics for categorical prediction<a name="canvas-categorical-prediction-metrics"></a>

The following defines the advanced metrics for categorical prediction in Amazon SageMaker Canvas and gives you information about how you can use them\.
+ Missing – A missing value contains no content or is non\-existent\. Missing values are automatically inferred\.
+ Mismatched – A mismatched value has a different data type from the type specified for its column\. SageMaker Canvas categorizes these values as missing and infers values for them\.
+ Unique – The number and percentage of values that are unique\.
+ Target correlation – A value between \-1 and 1 that represents strength of the linear relationship between a column and the target column\. `0` represents no detectable relationship\. `1` represents a strong positive relationship\. `-1` represents a strong negative relationship\.
+ Column impact – Identifies the relative impact of the column in predicting the target column\.

The following is a list of available metrics for binary classification\.
+ F1 – A balanced measure of accuracy that takes class balance into account\.
+ Accuracy – The percentage of correct predictions\.
+ Precision – Of all the times that \{category\-1\} was predicted, the prediction was correct \{precision\}% of the time\.
+ Recall – The model correctly predicted \{recall\}% to be \{category\-1\} when \{target\_column\} was actually \{category\-1\}\.
+ AUC – A value between 0 and 1 that indicates how well your model is able to separate the categories in your dataset\. A value of 1 indicates that it was able to separate the categories perfectly\.

The following is a list of available metrics for multi\-classification\.
+ F1 – A balanced measure of accuracy that takes class balance into account\.
+ Accuracy – The percentage of correct predictions\.
+ Precision – Of all the times that \{category\-1\} was predicted, the prediction was correct \{precision\}% of the time\.
+ Recall – The model correctly predicted \{recall\}% to be \{category\-1\} when \{target\_column\} was actually \{category\-1\}\.
+ AUC – A value between 0 and 1 that indicates how well your model is able to separate the categories in your dataset\. A value of 1 indicates that it was able to separate the categories perfectly\.
+ Average F1 – The F1 averaged for all categories\.
+ Average Accuracy – The percentage of correct predictions out of all the predictions that are made\.
+ Average Precision – The precision averaged for all categories\.
+ Average Recall – The recall averaged for all categories\.
+ Average AUC – The AUC averaged for all categories\.