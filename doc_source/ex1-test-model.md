# Step 6: Evaluate the Model<a name="ex1-test-model"></a>

Now that you have trained and deployed a model using Amazon SageMaker, evaluate the model to ensure that it generates accurate predictions on new data\. For model evaluation, use the test dataset that you created in [Step 3: Download, Explore, and Transform a Dataset](ex1-preprocess-data.md)\.

## Evaluate the Model Deployed to SageMaker Hosting Services<a name="ex1-test-model-endpoint"></a>

To evaluate the model and use it in production, invoke the endpoint with the test dataset and check whether the inferences you get returns a target accuracy you want to achieve\.

**To evaluate the model**

1. Set up the following function to predict each line of the test set\. In the following example code, the `rows` argument is to specify the number of lines to predict at a time\. You can change the value of it to perform a batch inference that fully utilizes the instance's hardware resource\.

   ```
   import numpy as np
   def predict(data, rows=1000):
       split_array = np.array_split(data, int(data.shape[0] / float(rows) + 1))
       predictions = ''
       for array in split_array:
           predictions = ','.join([predictions, xgb_predictor.predict(array).decode('utf-8')])
       return np.fromstring(predictions[1:], sep=',')
   ```

1. Run the following code to make predictions of the test dataset and plot a histogram\. You need to take only the feature columns of the test dataset, excluding the 0th column for the actual values\.

   ```
   import matplotlib.pyplot as plt
   
   predictions=predict(test.to_numpy()[:,1:])
   plt.hist(predictions)
   plt.show()
   ```  
![\[A histogram of predicted values.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-eval-predicted-values-histogram.png)

1. The predicted values are float type\. To determine `True` or `False` based on the float values, you need to set a cutoff value\. As shown in the following example code, use the Scikit\-learn library to return the output confusion metrics and classification report with a cutoff of 0\.5\.

   ```
   import sklearn
   
   cutoff=0.5
   print(sklearn.metrics.confusion_matrix(test.iloc[:, 0], np.where(predictions > cutoff, 1, 0)))
   print(sklearn.metrics.classification_report(test.iloc[:, 0], np.where(predictions > cutoff, 1, 0)))
   ```

   This should return the following confusion matrix:  
![\[An example of confusion matrix and statistics after getting the inference of the deployed model.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-evaluate-confusion-matrix.png)

1. To find the best cutoff with the given test set, compute the log loss function of the logistic regression\. The log loss function is defined as the negative log\-likelihood of a logistic model that returns prediction probabilities for its ground truth labels\. The following example code numerically and iteratively calculates the log loss values \(`-(y*log(p)+(1-y)log(1-p)`\), where `y` is the true label and `p` is a probability estimate of the corresponding test sample\. It returns a log loss versus cutoff graph\.

   ```
   import matplotlib.pyplot as plt
   
   cutoffs = np.arange(0.01, 1, 0.01)
   log_loss = []
   for c in cutoffs:
       log_loss.append(
           sklearn.metrics.log_loss(test.iloc[:, 0], np.where(predictions > c, 1, 0))
       )
   
   plt.figure(figsize=(15,10))
   plt.plot(cutoffs, log_loss)
   plt.xlabel("Cutoff")
   plt.ylabel("Log loss")
   plt.show()
   ```

   This should return the following log loss curve\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-evaluate-logloss-vs-cutoff.png)

1. Find the minimum points of the error curve using the NumPy `argmin` and `min` functions:

   ```
   print(
       'Log loss is minimized at a cutoff of ', cutoffs[np.argmin(log_loss)], 
       ', and the log loss value at the minimum is ', np.min(log_loss)
   )
   ```

   This should return: `Log loss is minimized at a cutoff of 0.53, and the log loss value at the minimum is 4.348539186773897`\.

   Instead of computing and minimizing the log loss function, you can estimate a cost function as an alternative\. For example, if you want to train a model to perform a binary classification for a business problem such as a customer churn prediction problem, you can set weights to the elements of confusion matrix and calculate the cost function accordingly\.

You have now trained, deployed, and evaluated your first model in SageMaker\.

**Tip**  
To monitor model quality, data quality, and bias drift, use Amazon SageMaker Model Monitor and SageMaker Clarify\. To learn more, see [Amazon SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html), [Monitor Data Quality](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-data-quality.html), [Monitor Model Quality](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-model-quality.html), [Monitor Bias Drift](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-model-monitor-bias-drift.html), and [Monitor Feature Attribution Drift](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-model-monitor-feature-attribution-drift.html)\.

**Tip**  
To get human review of low confidence ML predictions or a random sample of predictions, use Amazon Augmented AI human review workflows\. For more information, see [Using Amazon Augmented AI for Human Review](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-use-augmented-ai-a2i-human-review-loops.html)\.