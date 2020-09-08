# Compile a Model \(Amazon SageMaker SDK\)<a name="neo-job-compilation-sagemaker-sdk"></a>

Follow the steps described in the **Running the Training Job** section of the [MNIST Training, Compilation and Deployment with MXNet Module](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/mxnet_mnist/mxnet_mnist_neo.ipynb) sample to produce a machine learning model train using SageMaker\. Then you can use Neo to further optimize the model with the following code: 

```
output_path = ‘/’.join( mnist_estimator.output_path.split(‘/’)[:-1])
compiled_model = mnist_estimator.compile_model(target_instance_family='ml_c5', 
                                               input_shape={'data':[1, 784]},
                                               role=role,
                                               output_path=output_path)
```

This code compiles the model and saves the optimized model in `output_path`\. Sample notebooks of using SDK are provided in the [Neo Model Compilation Sample Notebooks](neo.md#neo-sample-notebooks) section\.