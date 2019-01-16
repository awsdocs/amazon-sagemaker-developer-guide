# Compile Machine Learning Models with the Amazon SageMaker SDK<a name="neo-job-compilation-sagemaker-sdk"></a>

Follow the steps described in the [Neo MXNet Part 1](https://docs.aws.amazon.com/sagemaker/latest/dg/mxnet-part1-train.html) sample to produce a machine learning model train using Amazon SageMaker\. Then you can use Neo to further optimize the model with the following code: 

```
output_path = ‘/’.join( mnist_estimator.output_path.split(‘/’)[:-1])
compiled_model = mnist_estimator.compile_model(target_instance_family='ml_c5', 
                                               input_shape={'data':[1, 784]},
                                               role=role,
                                               output_path=output_path)
```

This code compiles the model and saves the optimized model in `output_path`\. Sample notebooks of using SDK are provided in the [Amazon SageMaker Neo Sample Notebooks](neo.md#neo-sample-notebooks) section\.