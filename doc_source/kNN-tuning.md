# Tune a k\-NN Model<a name="kNN-tuning"></a>

The Amazon SageMaker k\-nearest neighbors algorithm is a supervised algorithm\. The algorithm consumes a test data set and emits a metric about the accuracy for a classification task or about the mean squared error for a regression task\. These accuracy metrics compare the model predictions for their respective task to the ground truth provided by the empirical test data\. To find the best model that reports the highest accuracy or lowest error on the test dataset, run a hyperparameter tuning job for k\-NN\. 

*Automatic model tuning*, also known as hyperparameter tuning, finds the best version of a model by running many jobs that test a range of hyperparameters on your dataset\. You choose the tunable hyperparameters, a range of values for each, and an objective metric\. You choose the objective metric appropriate for the prediction task of the algorithm\. Automatic model tuning searches the hyperparameters chosen to find the combination of values that result in the model that optimizes the objective metric\. The hyperparameters are used only to help estimate model parameters and are not used by the trained model to make predictions\.

For more information about model tuning, see [Perform Automatic Model Tuning](automatic-model-tuning.md)\.

## Metrics Computed by the k\-NN Algorithm<a name="km-metrics"></a>

The k\-nearest neighbors algorithm computes one of two metrics in the following table during training depending on the type of task specified by the `predictor_type` hyper\-parameter\. 
+ *classifier* specifies a classification task and computes `test:accuracy` 
+ *regressor* specifies a regression task and computes `test:mse`\.

Choose the `predictor_type` value appropriate for the type of task undertaken to calculate the relevant objective metric when tuning a model\.


| Metric Name | Description | Optimization Direction | 
| --- | --- | --- | 
| test:accuracy |  When `predictor_type` is set to *classifier*, k\-NN compares the predicted label, based on the average of the k\-nearest neighbors' labels, to the ground truth label provided in the test channel data\. The accuracy reported ranges from 0\.0 \(0%\) to 1\.0 \(100%\)\.  |  Maximize  | 
| test:mse |  When `predictor_type` is set to *regressor*, k\-NN compares the predicted label, based on the average of the k\-nearest neighbors' labels, to the ground truth label provided in the test channel data\. The mean squared error is computed by comparing the two labels\.  |  Minimize  | 

## Tunable k\-NN Hyperparameters<a name="km-tunable-hyperparameters"></a>

Tune the Amazon SageMaker k\-nearest neighbor model with the following hyperparameters\.


| Parameter Name | Parameter Type | Recommended Ranges | 
| --- | --- | --- | 
| k |  IntegerParameterRanges  |  MinValue: 1, MaxValue: 1024  | 
| sample\_size |  IntegerParameterRanges  |  MinValue: 256, MaxValue: 20000000  | 