# Model Scoring<a name="canvas-scoring"></a>

Amazon SageMaker Canvas provides different scoring information for both numeric and categorical prediction models\.

The **Scoring** section for a categorical prediction model gives you the ability to visualize all the predictions\. Line segments extend from the left of the page, indicating all the predictions the model has made\. In the middle of the page, the line segments converge on a perpendicular segment to indicate the proportion of each prediction to a single category\. From the predicted category, the segments branch out to the actual category\. You can get a visual sense of how accurate the predictions were by following each line segment from the predicted category to the actual category\.

The following image gives you an example **Scoring section** for a **2 category prediction** model\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-binary-classification.png)

The following image gives you an example **Scoring** section for a **3\+ category prediction** model\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-multiclass-classification.png)

The **Scoring** section for numeric prediction shows a line to indicate the model's predicted value in relation to the data used to make predictions\. The values of the numeric prediction are often \+/\- the RMSE \(root mean squared error\) value\. The value that the model predicts is often within the range of the RMSE\. The width of the purple band around the line indicates the RMSE range\. The predicted values often fall within the range\.

The following image shows the **Scoring** section for numeric prediction\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-analyze/canvas-analyze-regression-scoring.png)