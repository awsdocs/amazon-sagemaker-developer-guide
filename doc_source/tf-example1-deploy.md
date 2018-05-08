# Step 3: Deploy the Trained Model<a name="tf-example1-deploy"></a>

To deploy the model, use Amazon SageMaker hosting services\. During deployment, Amazon SageMaker launches the ML compute instances and deploys the model \(the model artifacts and inference code\) on them\. In response, you get an endpoint\. To get inferences from the model, your application sends requests to the endpoint\. 

For fast deployment, use the `deploy` method in the `sagemaker.tensorflow.TensorFlow` class\. The class is included in the high\-level Python library provided by Amazon SageMaker\. The `deploy` method does the following: 

1. Creates an Amazon SageMaker model by calling the [CreateModel](API_CreateModel.md) API\. The model contains important information, such as the location of the model artifacts and the inference code image\. 

1. Creates an endpoint configuration by calling the [CreateEndpointConfig](API_CreateEndpointConfig.md) API\. This configuration specifies the name of the model \(which was created in the preceding step\), and the resource configuration \(the type and number of ML compute instances to launch for hosting\)\.

1. Creates the endpoint by calling the [CreateEndpoint](API_CreateEndpoint.md) API and specifying the endpoint configuration\. Amazon SageMaker launches ML compute instances as specified in the endpoint configuration, and deploys the model on them\. 

To deploy the model, copy, paste, and run the following code:

```
%%time

iris_predictor = iris_estimator.deploy(initial_instance_count=1,
                                       instance_type='ml.m4.xlarge')
```

When the status of the endpoint is INSERVICE, your model had been deployed\. The API returns a `TensorFlowPredictor` object\. To get inferences, you will use the `predict` method of this object\.

**Note**  
You can deploy your model to an endpoint hosted on your local computer by specifying `local` as the value for `train_instance_type` and `1` as the value for `train_instance_count`\. For more information about local mode, see [https://github.com/aws/sagemaker-python-sdk#local-mode](https://github.com/aws/sagemaker-python-sdk#local-mode) in the *Amazon SageMaker Python SDK*\.

## Next Step<a name="tf-example1-deploy-nexttopic"></a>

 [Step 4: Invoke the Endpoint to Get Inferences](tf-example1-invoke.md) 