# Deploy a Model Compiled with Neo \(Amazon SageMaker SDK\)<a name="neo-deployment-hosting-services-sdk"></a>

The object handle for the compiled model supplies the `deploy` function, which allows you to create an endpoint to serve inference requests\. The function lets you set the number and type of instances that are used for the endpoint\. You must choose an instance for which you have compiled your model\. For example, in the job compiled in [Compile a Model \(Amazon SageMaker SDK\)](neo-job-compilation-sagemaker-sdk.md) section, this is `ml_c5`\. The Neo API uses a special runtime, the *Neo runtime*, to run Neo\-optimized models\.

```
predictor = compiled_model.deploy(initial_instance_count = 1, instance_type = 'ml.c5.4xlarge')
```

After the command is done, the name of the newly created endpoint is printed in the Jupyter notebook\.