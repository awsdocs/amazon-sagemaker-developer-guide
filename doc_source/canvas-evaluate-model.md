# Evaluating Your Model's Performance in Amazon SageMaker Canvas<a name="canvas-evaluate-model"></a>

You can evaluate how well your model performed on your data before you start using it to make predictions by using the following:
+ Column impact
+ Scoring
+ Advanced metrics

**Column impact** is a percentage score that indicates how much weight a column has in making predictions in relation to the other columns\. For a column impact of 25%, SageMaker Canvas weighs the prediction as 25% for the column and 75% for the other columns\.

**Scoring** is a section that shows visualizations and figures that you can use to get more insights into your model's performance beyond the overall accuracy metric\. For a categorical prediction, you can see the predicted values in contrast to the actual values\.

**Advanced metrics** is a section that contains information that you can use for a deeper understanding of your model's performance\.