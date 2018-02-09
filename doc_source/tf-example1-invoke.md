# Step 4: Invoke the Endpoint to Get Inferences<a name="tf-example1-invoke"></a>

To get inferences, send requests using the `predict` method of the `TensorFlowPredictor` object\. \(This object was returned in Step 3\)\. The `predict` method calls the Amazon SageMaker [InvokeEndpoint](API_runtime_InvokeEndpoint.md)\. 

 Run the `predict` method: 

```
iris_predictor.predict([6.4, 3.2, 4.5, 1.5]) #expected label to be 1
```

For more information about the input features, see [About the Training Dataset](tf-example1.md#tf-script-own-algo-ex1-about-dataset)\. 

The model predicts that the flower species is Iris versicolor \(label 1\) with a probability of 97%:

```
{u'result': {u'classifications': [{u'classes': [{u'label': u'0',
      u'score': 0.009605729021131992},
     {u'label': u'1', u'score': 0.9699361324310303},
     {u'label': u'2', u'score': 0.02045818418264389}]}]}}
```

Your Amazon SageMaker notebook instance includes additional examples\.