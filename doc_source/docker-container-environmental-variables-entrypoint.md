# Environmental Variables used by Amazon SageMaker Containers to Define Entry Points<a name="docker-container-environmental-variables-entrypoint"></a>

When creating a Dockerfile, you must define an entry point that specifies the location of the code to run when the container starts\. Amazon SageMaker Containers does this by setting an `ENV` environment variable\. The environment variable that you need to set depends on the job you want to do: 
+ To run a script,specify the `SAGEMAKER_PROGRAM` `ENV` variable\.
+ To train an algorithm, specify the `SAGEMAKER_TRAINING_MODULE` `ENV` variable\.
+ To host a model, specify the `SAGEMAKER_SERVING_MODULE` `ENV` variable\.

You can use the Amazon SageMaker containers SDK package to set environment variables\. 

`SAGEMAKER_PROGRAM`

Train scripts similar to those you would use outside Amazon SageMaker using Amazon SageMaker Script Mode\. It supports Python and Shell scripts: Amazon SageMaker uses the Python interpreter for any script with the `.py` suffix\.Amazon SageMaker uses the Shell interpreter to execute any other script\.

When running a program to specify the entry point for Script Mode, set the `SAGEMAKER_PROGRAM` environmental variable\. The script must be located in the `/opt/ml/code` folder\. 

For example, the container used in the example in [Get Started: Use Amazon SageMaker Containers to Run a Python Script](build-container-to-train-script-get-started.md) sets this `ENV` as follows\.

```
ENV SAGEMAKER_PROGRAM train.py
```

 The Amazon SageMaker [PyTorch](https://github.com/aws/sagemaker-mxnet-container/blob/master/docker/1.3.0/final/Dockerfile.cpu#L63) container sets the `ENV` variable as follows\.

```
ENV SAGEMAKER_PROGRAM cifar10.py
```

In the example, cifar10\.py is the program that implements the training algorithm and handles loading the model for inferences\. For more information, see the [Extending our PyTorch containers](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/pytorch_extending_our_containers/pytorch_extending_our_containers.ipynb) notebook\. 

`SAGEMAKER_TRAINING_MODULE`

When training an algorithm, specify the location of the module that contains the training logic by setting the `SAGEMAKER_TRAINING_MODULE` environment variable\. An Amazon SageMaker container invokes this module when the container starts training\. For example, you set this environment variable in [MXNet](https://github.com/aws/sagemaker-mxnet-container/blob/master/docker/1.3.0/final/Dockerfile.cpu#L63) as follows\.

```
ENV SAGEMAKER_TRAINING_MODULE sagemaker_mxnet_container.training:main
```

For [TensorFlow](https://github.com/aws/sagemaker-tensorflow-container/blob/script-mode/docker/1.12.0/Dockerfile.cpu#92), set this `environmant variable` as follows\.

```
ENV SAGEMAKER_TRAINING_MODULE sagemaker_tensorflow_container.training:main
```

The code that implements this logic is in [Amazon SageMaker Containers](https://github.com/aws/sagemaker-containers/blob/master/src/sagemaker_containers/_trainer.py)\.

`SAGEMAKER_SERVING_MODULE`

To locate the module that contains the hosting logic when deploying a model, set the `SAGEMAKER_SERVING_MODULE` environmental variable\. An Amazon SageMaker container invokes this module when it starts hosting\. For [MXNet](https://github.com/aws/sagemaker-mxnet-container/blob/master/docker/1.3.0/final/Dockerfile), set this environment as follows\.

```
ENV SAGEMAKER_SERVING_MODULE sagemaker_mxnet_container.serving:main
```

The code that implements this logic is in the: [Amazon SageMaker Containers](https://github.com/aws/sagemaker-containers/blob/master/src/sagemaker_containers/_server.py)\.