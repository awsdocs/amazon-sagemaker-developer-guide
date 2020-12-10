# Compile a Model \(Amazon SageMaker SDK\)<a name="neo-job-compilation-sagemaker-sdk"></a>

 You can use the [https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html?#sagemaker.estimator.Estimator.compile_model](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html?#sagemaker.estimator.Estimator.compile_model) API in the [Amazon SageMaker SDK for Python](https://sagemaker.readthedocs.io/en/stable/) to compile a trained model and optimize it for specific target hardware\. The API should be invoked on the estimator object used during model training\. 

**Note**  
You must set `MMS_DEFAULT_RESPONSE_TIMEOUT` environment variable to `500` when compiling the model with MXNet or PyTorch\. The environment variable is not needed for TensorFlow\. 

 The following is an example of how you can compile a model using the `trained_model_estimator` object: 

```
# Replace the value of expected_trained_model_input below and
# specify the name & shape of the expected inputs for your trained model
# in json dictionary form
expected_trained_model_input = {'data':[1, 784]}

# Replace the example target_instance_family below to your preferred target_instance_family
compiled_model = trained_model_estimator.compile_model(target_instance_family='ml_c5',
        input_shape=expected_trained_model_input,
        output_path='insert s3 output path',
        env={'MMS_DEFAULT_RESPONSE_TIMEOUT': '500'})
```

The code compiles the model, saves the optimized model at `output_path`, and creates a SageMaker model that can be deployed to an endpoint\. Sample notebooks of using the SDK for Python are provided in the [Neo Model Compilation Sample Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html#neo-sample-notebooks) section\. 