# Use Debugger with Custom Training Containers<a name="debugger-bring-your-own-container"></a>

Amazon SageMaker Debugger is available for any deep learning models that you bring to Amazon SageMaker\. The AWS CLI, SageMaker `Estimator` API, and the Debugger APIs enable you to use any Docker base images to build and customize containers to train your models\. To use Debugger with customized containers, you need to make a minimal change to your training script to implement the Debugger hook callback and retrieve tensors from training jobs\.

You need the following resources to build a customized container with Debugger\.
+ [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)
+ [The SMDebug open source client library](https://github.com/awslabs/sagemaker-debugger)
+ A Docker base image of your choice
+ Your training script with a Debugger hook registered – For more information about registering a Debugger hook to your training script, see [Register Debugger Hook to Your Training Script](#debugger-script-mode)\.

For an end\-to\-end example of using Debugger with a custom training container, see the following example notebook\.
+ [Build a Custom Training Container and Debug Training Jobs with Debugger](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/build_your_own_container_with_debugger/debugger_byoc.html)

**Tip**  
This custom container with Debugger guide is an extension of the [Adapting Your Own Training Container](adapt-training-container.md) guide which walks you thorough how to build and push your custom training container to Amazon ECR\.

## Prepare to Build a Custom Training Container<a name="debugger-bring-your-own-container-1"></a>

To build a docker container, the basic structure of files should look like the following:

```
├── debugger_custom_container_test_notebook.ipynb      # a notebook to run python snippet codes
└── debugger_custom_container_test_folder              # this is a docker folder
    ├──  your-training-script.py                       # your training script with Debugger hook
    └──  Dockerfile                                    # a Dockerfile to build your own container
```

## Register Debugger Hook to Your Training Script<a name="debugger-script-mode"></a>

To debug your model training, you need to add a Debugger hook to your training script\.

**Note**  
This step is required to collect model parameters \(output tensors\) for debugging your model training\. If you only want to monitor and profile, you can skip this hook registration step and exclude the `debugger_hook_config` parameter when constructing an estimater\.

The following example code shows the structure of a training script using the Keras ResNet50 model and how to pass the Debugger hook as a Keras callback for debugging\. To find a complete training script, see [TensorFlow training script with SageMaker Debugger hook](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-debugger/build_your_own_container_with_debugger/docker/tf_keras_resnet_byoc.py)\.

```
# An example of training script (your-training-script.py)
import tensorflow.compat.v2 as tf
from tensorflow.keras.applications.resnet50 import ResNet50
import smdebug.tensorflow as smd

def train(batch_size, epoch, model, hook):

    ...
    model.fit(X_train, Y_train,
              batch_size=batch_size,
              epochs=epoch,
              validation_data=(X_valid, Y_valid),
              shuffle=True,

              # smdebug modification: Pass the Debugger hook in the main() as a Keras callback
              callbacks=[hook])

def main():
    parser=argparse.ArgumentParser(description="Train resnet50 cifar10")

    # hyperparameter settings
    parser.add_argument(...)
    
    args = parser.parse_args()

    model=ResNet50(weights=None, input_shape=(32,32,3), classes=10)

    # Add the following line to register the Debugger hook for Keras.
    hook=smd.KerasHook.create_from_json_file()

    # Start the training.
    train(args.batch_size, args.epoch, model, hook)

if __name__ == "__main__":
    main()
```

For more information about registering the Debugger hook for the supported frameworks and algorithm, see the following links in the SMDebug client library:
+ [SMDebug TensorFlow hook](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/tensorflow.md)
+ [SMDebug PyTorch hook](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/pytorch.md)
+ [SMDebug MXNet hook](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/mxnet.md)
+ [SMDebug XGBoost hook](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/xgboost.md)

In the following example notebooks' training scripts, you can find more examples about how to add the Debugger hooks to training scripts and collect output tensors in detail:
+ [ Debugger in script mode with the TensorFlow 2\.1 framework](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/tensorflow2/tensorflow2_keras_custom_container/tf2-keras-custom-container.html)

  To see the difference between using Debugger in a Deep Learning Container and in script mode, open this notebook and put it and [ the previous Debugger in a Deep Learning Container TensorFlow v2\.1 notebook example](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/tensorflow2/tensorflow2_zero_code_change/tf2-keras-default-container.html) side by side\. 

   In script mode, the hook configuration part is removed from the script in which you set the estimator\. Instead, the Debugger hook feature is merged into the training script, [ TensorFlow Keras ResNet training script in script mode](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow2/tensorflow2_keras_custom_container/src/tf_keras_resnet_byoc.py)\. The training script imports the `smdebug` library in the required TensorFlow Keras environment to communicate with the TensorFlow ResNet50 algorithm\. It also manually implements the `smdebug` hook functionality by adding the `callbacks=[hook]` argument inside the `train` function \(in line 49\), and by adding the manual hook configuration \(in line 89\) provided through SageMaker Python SDK\.

  This script mode example runs the training job in the TF 2\.1 framework for direct comparison with the zero script change in the TF 2\.1 example\. The benefit of setting up Debugger in script mode is the flexibility to choose framework versions not covered by AWS Deep Learning Containers\. 
+ [ Using Amazon SageMaker Debugger in a PyTorch Container in Script Mode ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_custom_container)

  This notebook enables Debugger in script mode in PyTorch v1\.3\.1 framework\. PyTorch v1\.3\.1 is supported by SageMaker containers, and this example shows details of how to modify a training script\. 

  The SageMaker PyTorch estimator is already in script mode by default\. In the notebook, the line to activate `script_mode` is not included in the estimator configuration\.

  This notebook shows detailed steps to change [ an original PyTorch training script](https://github.com/pytorch/examples/blob/master/mnist/main.py) to [ a modified version with Debugger enabled](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/pytorch_custom_container/scripts/pytorch_mnist.py)\. Additionally, this example shows how you can use Debugger built\-in rules to detect training issues such as the vanishing gradients problem, and the Debugger trial features to call and analyze the saved tensors\. 

## Create and Configure a Dockerfile<a name="debugger-bring-your-own-container-2"></a>

Open your SageMaker JupyterLab and create a new folder, `debugger_custom_container_test_folder` in this example, to save your training script and `Dockerfile`\. The following code example is a `Dockerfile` that includes essential docker build commends\. Paste the following code into the `Dockerfile` text file and save it\. Upload your training script to the same folder\.

```
# Specify a docker base image
FROM tensorflow/tensorflow:2.2.0rc2-gpu-py3
RUN /usr/bin/python3 -m pip install --upgrade pip
RUN pip install --upgrade protobuf

# Install required packages to enable the SageMaker Python SDK and the smdebug library
RUN pip install sagemaker-training
RUN pip install smdebug
CMD ["bin/bash"]
```

If you want to use a pre\-built AWS Deep Learning Container image, see [Available AWS Deep Learning Containers Images](https://aws.amazon.com/releasenotes/available-deep-learning-containers-images/)\.

## Build and Push the Custom Training Container to Amazon ECR<a name="debugger-bring-your-own-container-3"></a>

Create a test notebook, `debugger_custom_container_test_notebook.ipynb`, and run the following code in the notebook cell\. This will access the `debugger_byoc_test_docker` directory, build the docker with the specified `algorithm_name`, and push the docker container to your Amazon ECR\.

```
import boto3

account_id = boto3.client('sts').get_caller_identity().get('Account')
ecr_repository = 'sagemaker-debugger-mnist-byoc-tf2'
tag = ':latest'

region = boto3.session.Session().region_name

uri_suffix = 'amazonaws.com'
if region in ['cn-north-1', 'cn-northwest-1']:
    uri_suffix = 'amazonaws.com.cn'
byoc_image_uri = '{}.dkr.ecr.{}.{}/{}'.format(account_id, region, uri_suffix, ecr_repository + tag)

!docker build -t $ecr_repository docker
!$(aws ecr get-login --region $region --registry-ids $account_id --no-include-email)
!aws ecr create-repository --repository-name $ecr_repository
!docker tag {ecr_repository + tag} $byoc_image_uri
!docker push $byoc_image_uri
```

**Tip**  
If you use one of the AWS Deep Learning Container base images, run the following code to log in to Amazon ECR and access to the Deep Learning Container image repository\.  

```
! aws ecr get-login-password --region {region} | docker login --username AWS --password-stdin 763104351884.dkr.ecr.us-east-1.amazonaws.com
```

## Run and Debug Training Jobs Using the Custom Training Container<a name="debugger-bring-your-own-container-4"></a>

After you build and push your docker container to Amazon ECR, configure a SageMaker estimator with your training script and the Debugger\-specific parameters\. After you execute the `estimator.fit()`, Debugger will collect output tensors, monitor them, and detect training issues\. Using the saved tensors, you can further analyze the training job by using the `smdebug` core features and tools\. Configuring a workflow of Debugger rule monitoring process with Amazon CloudWatch Events and AWS Lambda, you can automate a stopping training job process whenever the Debugger rules spots training issues\.

```
import sagemaker
from sagemaker.estimator import Estimator
from sagemaker.debugger import Rule, DebuggerHookConfig, CollectionConfig, rule_configs

profiler_config=ProfilerConfig(...)
debugger_hook_config=DebuggerHookConfig(...)
rules=[
    Rule.sagemaker(rule_configs.built_in_rule()),
    ProfilerRule.sagemaker(rule_configs.BuiltInRule())
]

estimator=Estimator(
    image_uri=byoc_image_uri,
    entry_point="./debugger_custom_container_test_folder/your-training-script.py"
    role=sagemaker.get_execution_role(),
    base_job_name='debugger-custom-container-test',
    instance_count=1,
    instance_type='ml.p3.2xlarge',
    
    # Debugger-specific parameters
    profiler_config=profiler_config,
    debugger_hook_config=debugger_hook_config,
    rules=rules
)

# start training
estimator.fit()
```