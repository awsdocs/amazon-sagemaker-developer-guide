# Run TensorFlow Training Jobs with SageMaker Training Compiler<a name="training-compiler-enable-tensorflow"></a>

You can use any of the SageMaker interfaces to run a training job with SageMaker Training Compiler: Amazon SageMaker Studio, Amazon SageMaker notebook instances, AWS SDK for Python \(Boto3\), and AWS Command Line Interface\.

**Topics**
+ [Using the SageMaker Python SDK](#training-compiler-enable-tensorflow-pysdk)
+ [Using the SageMaker Python SDK and Extending SageMaker Framework Deep Learning Containers](#training-compiler-enable-tensorflow-sdk-extend-container)
+ [Enable SageMaker Training Compiler Using the SageMaker `CreateTrainingJob` API Operation](#training-compiler-enable-tensorflow-api)

## Using the SageMaker Python SDK<a name="training-compiler-enable-tensorflow-pysdk"></a>

To turn on SageMaker Training Compiler, add the `compiler_config` parameter to the SageMaker TensorFlow or Hugging Face estimator\. Import the `TrainingCompilerConfig` class and pass an instance of it to the `compiler_config` parameter\. The following code examples show the structure of the SageMaker estimator classes with SageMaker Training Compiler turned on\.

**Tip**  
To get started with prebuilt models provided by the TensorFlow and Transformers libraries, try using the batch sizes provided in the reference table at [Tested Models](training-compiler-support.md#training-compiler-tested-models)\.

**Note**  
SageMaker Training Compiler for TensorFlow is available through the SageMaker [TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) and [Hugging Face](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/sagemaker.huggingface.html#hugging-face-estimator) framework estimators\.

For information that fits your use case, see one of the following options\.

### For single GPU training<a name="training-compiler-estimator-tensorflow-single"></a>

------
#### [ TensorFlow ]

```
from sagemaker.tensorflow import TensorFlow, TrainingCompilerConfig

# the original max batch size that can fit into GPU memory without compiler
batch_size_native=12
learning_rate_native=float('5e-5')

# an updated max batch size that can fit into GPU memory with compiler
batch_size=64    

# update the global learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size

hyperparameters={
    "n_gpus": 1,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

tensorflow_estimator=TensorFlow(
    entry_point='train.py',
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    framework_version='2.9.1',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

tensorflow_estimator.fit()
```

To prepare your training script, see the following pages\.
+ [For single GPU training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-keras-single-gpu) of a model constructed using TensorFlow Keras \(`tf.keras.*`\)\.
+ [For single GPU training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-no-keras-single-gpu) of a model constructed using TensorFlow modules \(`tf.*` excluding the TensorFlow Keras modules\)\.

------
#### [ Hugging Face Estimator with TensorFlow ]

```
from sagemaker.huggingface import HuggingFace, TrainingCompilerConfig

# the original max batch size that can fit into GPU memory without compiler
batch_size_native=12
learning_rate_native=float('5e-5')

# an updated max batch size that can fit into GPU memory with compiler
batch_size=64

# update the global learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size

hyperparameters={
    "n_gpus": 1,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

tensorflow_huggingface_estimator=HuggingFace(
    entry_point='train.py',
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    transformers_version='4.21.1',
    tensorflow_version='2.6.3',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

tensorflow_huggingface_estimator.fit()
```

To prepare your training script, see the following pages\.
+ [For single GPU training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-keras-single-gpu) of a TensorFlow Keras model with Hugging Face Transformers
+ [For single GPU training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-no-keras-single-gpu) of a TensorFlow model with Hugging Face Transformers

------

### For distributed training<a name="training-compiler-estimator-tensorflow-distributed"></a>

------
#### [ Hugging Face Estimator with TensorFlow ]

```
from sagemaker.huggingface import HuggingFace, TrainingCompilerConfig

# choose an instance type, specify the number of instances you want to use,
# and set the num_gpus variable the number of GPUs per instance.
instance_count=1
instance_type='ml.p3.8xlarge'
num_gpus=4

# the original max batch size that can fit to GPU memory without compiler
batch_size_native=16
learning_rate_native=float('5e-5')

# an updated max batch size that can fit to GPU memory with compiler
batch_size=26

# update learning rate
learning_rate=learning_rate_native/batch_size_native*batch_size*num_gpus*instance_count

hyperparameters={
    "n_gpus": num_gpus,
    "batch_size": batch_size,
    "learning_rate": learning_rate
}

tensorflow_huggingface_estimator=HuggingFace(
    entry_point='train.py',
    instance_count=instance_count,
    instance_type=instance_type,
    transformers_version='4.21.1',
    tensorflow_version='2.6.3',
    hyperparameters=hyperparameters,
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

tensorflow_huggingface_estimator.fit()
```

**Tip**  
To prepare your training script, see the following pages\.  
[For distributed training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-keras-distributed) of a TensorFlow Keras model with Hugging Face Transformers
[For distributed training](training-compiler-tensorflow-models.md#training-compiler-tensorflow-models-transformers-no-keras-distributed) of a TensorFlow model with Hugging Face Transformers

------

The following list is the minimal set of parameters required to run a SageMaker training job with the compiler\.

**Note**  
When using the SageMaker Hugging Face estimator, you must specify the `transformers_version`, `tensorflow_version`, `hyperparameters`, and `compiler_config` parameters to enable SageMaker Training Compiler\. You cannot use `image_uri` to manually specify the Training Compiler integrated Deep Learning Containers that are listed at [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
+ `entry_point` \(str\) – Required\. Specify the file name of your training script\.
+ `instance_count` \(int\) – Required\. Specify the number of instances\.
+ `instance_type` \(str\) – Required\. Specify the instance type\.
+ `transformers_version` \(str\) – Required only when using the SageMaker Hugging Face estimator\. Specify the Hugging Face Transformers library version supported by SageMaker Training Compiler\. To find available versions, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
+ `framework_version` or `tensorflow_version` \(str\) – Required\. Specify the TensorFlow version supported by SageMaker Training Compiler\. To find available versions, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.
**Note**  
When using the SageMaker TensorFlow estimator, you must specify `framework_version`\.  
When using the SageMaker Hugging Face estimator, you must specify both `transformers_version` and `tensorflow_version`\.
+ `hyperparameters` \(dict\) – Optional\. Specify hyperparameters for the training job, such as `n_gpus`, `batch_size`, and `learning_rate`\. When you enable SageMaker Training Compiler, try larger batch sizes and adjust the learning rate accordingly\. To find case studies of using the compiler and adjusted batch sizes to improve training speed, see [Tested Models](training-compiler-support.md#training-compiler-tested-models) and [SageMaker Training Compiler Example Notebooks and Blogs](training-compiler-examples-and-blogs.md)\.
+ `compiler_config` \(TrainingCompilerConfig object\) – Required\. Include this parameter to turn on SageMaker Training Compiler\. The following are parameters for the `TrainingCompilerConfig` class\.
  + `enabled` \(bool\) – Optional\. Specify `True` or `False` to turn on or turn off SageMaker Training Compiler\. The default value is `True`\.
  + `debug` \(bool\) – Optional\. To receive more detailed training logs from your compiler\-accelerated training jobs, change it to `True`\. However, the additional logging might add overhead and slow down the compiled training job\. The default value is `False`\.

**Warning**  
If you turn on SageMaker Debugger, it might impact the performance of SageMaker Training Compiler\. We recommend that you turn off Debugger when running SageMaker Training Compiler to make sure there's no impact on performance\. For more information, see [Considerations](training-compiler-tips-pitfalls.md#training-compiler-tips-pitfalls-considerations)\. To turn the Debugger functionalities off, add the following two arguments to the estimator:  

```
disable_profiler=True,
debugger_hook_config=False
```

If the training job with the compiler is launched successfully, you receive the following logs during the job initialization phase: 
+ With `TrainingCompilerConfig(debug=False)`

  ```
  Found configuration for Training Compiler
  Configuring SM Training Compiler...
  ```
+ With `TrainingCompilerConfig(debug=True)`

  ```
  Found configuration for Training Compiler
  Configuring SM Training Compiler...
  Training Compiler set to debug mode
  ```

## Using the SageMaker Python SDK and Extending SageMaker Framework Deep Learning Containers<a name="training-compiler-enable-tensorflow-sdk-extend-container"></a>

AWS Deep Learning Containers \(DLC\) for TensorFlow use adapted versions of TensorFlow that include changes on top of the open\-source TensorFlow framework\. The [SageMaker Framework Deep Learning Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#sagemaker-framework-containers-sm-support-only) are optimized for the underlying AWS infrastructure and Amazon SageMaker\. With the advantage of using the DLCs, SageMaker Training Compiler integration adds more performance improvements over the native TensorFlow\. Furthermore, you can create a custom training container by extending the DLC image\.

**Note**  
This Docker customization feature is currently available only for TensorFlow\.

To extend and customize the SageMaker TensorFlow DLCs for your use\-case, use the following instructions\.

### Create a Dockerfile<a name="training-compiler-enable-tensorflow-sdk-extend-container-create-dockerfile"></a>

Use the following Dockerfile template to extend the SageMaker TensorFlow DLC\. You must use the SageMaker TensorFlow DLC image as the base image of your Docker container\. To find the SageMaker TensorFlow DLC image URIs, see [Supported Frameworks](https://docs.aws.amazon.com/sagemaker/latest/dg/training-compiler-support.html#training-compiler-supported-frameworks)\.

```
# SageMaker TensorFlow Deep Learning Container image
FROM 763104351884.dkr.ecr.<aws-region>.amazonaws.com/tensorflow-training:<image-tag>

ENV PATH="/opt/ml/code:${PATH}"

# This environment variable is used by the SageMaker container 
# to determine user code directory.
ENV SAGEMAKER_SUBMIT_DIRECTORY /opt/ml/code

# Add more code lines to customize for your use-case
...
```

For more information, see [Step 2: Create and upload the Dockerfile and Python training scripts](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html#byoc-training-step2)\.

Consider the following pitfalls when extending SageMaker Framework DLCs:
+ Do not explicitly uninstall or change the version of TensorFlow packages in SageMaker containers\. Doing so causes the AWS optimized TensorFlow packages to be overwritten by open\-source TensorFlow packages, which might result in performance degradation\.
+ Watch out for packages that have a particular TensorFlow version or flavor as a dependency\. These packages might implicitly uninstall the AWS optimized TensorFlow and install open\-source TensorFlow packages\.

For example, there’s a known issue that the [tensorflow/models](https://github.com/tensorflow/models) and [tensorflow/text](https://github.com/tensorflow/text) libraries always attempt to [reinstall open source TensorFlow](https://github.com/tensorflow/models/issues/9267)\. If you need to install these libraries to choose a specific version for your use case, we recommend that you look into the SageMaker TensorFlow DLC Dockerfiles for v2\.9 or later\. The paths to the Dockerfiles are typically in the following format: `tensorflow/training/docker/<tensorflow-version>/py3/<cuda-version>/Dockerfile.gpu`\. In the Dockerfiles, you should find the code lines to reinstall AWS managed TensorFlow binary \(specified to the `TF_URL` environment variable\) and other dependencies in order\. The reinstallation section should look like the following example:

```
# tf-models does not respect existing installations of TensorFlow 
# and always installs open source TensorFlow

RUN pip3 install --no-cache-dir -U \
    tf-models-official==x.y.z

RUN pip3 uninstall -y tensorflow tensorflow-gpu \
  ; pip3 install --no-cache-dir -U \
    ${TF_URL} \
    tensorflow-io==x.y.z \
    tensorflow-datasets==x.y.z
```

### Build and push to ECR<a name="training-compiler-enable-tensorflow-sdk-extend-container-build-and-push"></a>

To build and push your Docker container to Amazon ECR, follow the instructions in the following links:
+ [Step 3: Build the container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html#byoc-training-step3)
+ [Step 4: Test the container](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html#byoc-training-step4)
+ [Step 5: Push the container to Amazon ECR](https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html#byoc-training-step5)

### Run using the SageMaker Python SDK Estimator<a name="training-compiler-enable-tensorflow-sdk-extend-container-run-job"></a>

Use the SageMaker TensorFlow framework estimator as usual\. You must specify `image_uri` to use the new container you hosted in Amazon ECR\.

```
import sagemaker, boto3
from sagemaker import get_execution_role
from sagemaker.tensorflow import TensorFlow, TrainingCompilerConfig

account_id = boto3.client('sts').get_caller_identity().get('Account')
ecr_repository = 'tf-custom-container-test'
tag = ':latest'

region = boto3.session.Session().region_name

uri_suffix = 'amazonaws.com'

byoc_image_uri = '{}.dkr.ecr.{}.{}/{}'.format(
    account_id, region, uri_suffix, ecr_repository + tag
)

byoc_image_uri
# This should return something like
# 111122223333.dkr.ecr.us-east-2.amazonaws.com/tf-custom-container-test:latest

estimator = TensorFlow(
    image_uri=image_uri,
    role=get_execution_role(),
    base_job_name='tf-custom-container-test-job',
    instance_count=1,
    instance_type='ml.p3.8xlarge'
    compiler_config=TrainingCompilerConfig(),
    disable_profiler=True,
    debugger_hook_config=False
)

# Start training
estimator.fit()
```

## Enable SageMaker Training Compiler Using the SageMaker `CreateTrainingJob` API Operation<a name="training-compiler-enable-tensorflow-api"></a>

SageMaker Training Compiler configuration options must be specified through the `AlgorithmSpecification` and `HyperParameters` field in the request syntax for the [`CreateTrainingJob` API operation](http://amazonaws.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\.

```
"AlgorithmSpecification": {
    "TrainingImage": "<sagemaker-training-compiler-enabled-dlc-image>"
},

"HyperParameters": {
    "sagemaker_training_compiler_enabled": "true",
    "sagemaker_training_compiler_debug_mode": "false"
}
```

To find a complete list of deep learning container image URIs that have SageMaker Training Compiler implemented, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\.