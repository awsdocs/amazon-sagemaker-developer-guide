# Step 3 : Deploy the Trained Model<a name="mxnet-example1-deploy"></a>

You can use Amazon SageMaker hosting services to deploy the model\. During deployment, Amazon SageMaker launches ML compute instances and deploys the model \(model artifacts and inference code\) on them\. In response, you get an endpoint\. To get inferences from the model, your application code can send requests to the endpoint\.

The `sagemaker.mxnet.MXNet` class in the high\-level Python library provides the `deploy` method for quickly deploying your model\. The `deploy` method does the following in order:

1. Creates an Amazon SageMaker model by calling the [CreateModel](API_CreateModel.md) API\. The model that you create in Amazon SageMaker holds information such as location of the model artifacts and the inference code image\. 

1. Creates an endpoint configuration by calling the [CreateEndpointConfig](API_CreateEndpointConfig.md) API\. This configuration holds necessary information including the name of the model \(which was created in the preceding step\) and the resource configuration \(the type and number of ML compute instances to launch for hosting\)\. 

1. Creates the endpoint by calling the [CreateEndpoint](API_CreateEndpoint.md) API and specifying the endpoint configuration created in the preceding step\. Amazon SageMaker launches ML compute instances as specified in the endpoint configuration, and deploys the model on them\.

Copy, paste, and run the following code:

```
%%time

predictor = mnist_estimator.deploy(initial_instance_count=1,
                                   instance_type='ml.m4.xlarge')
```

When the status of the endpoint is INSERVICE the API returns an `MXNetPredictor` object\. Use the `predict` method of this object to obtain inferences\.

**Note**  
You can deploy a model to an endpoint hosted on your local computer by specifying `local` as the value for `train_instance_type` and `1` as the value for `train_instance_count`\. For more information about local mode, see [https://github.com/aws/sagemaker-python-sdk#local-mode](https://github.com/aws/sagemaker-python-sdk#local-mode) in the *Amazon SageMaker Python SDK*\.

## Next Step<a name="mxnet-example1-deploy-nexttopic"></a>

 [Step 4 : Invoke the Endpoint to Get Inferences ](mxnet-example-invoke.md) 