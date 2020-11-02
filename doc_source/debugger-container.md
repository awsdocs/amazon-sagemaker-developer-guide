# Use Debugger in AWS Containers<a name="debugger-container"></a>

 To enable Amazon SageMaker Debugger, use the pre\-built AWS containers supported by Deep Learning Containers and the XGBoost built\-in algorithm\. Using the AWS containers with the Debugger features pre\-installed, you can simply use your training script with no change or only minimal changes depending on your choice of frameworks\.

You can use Amazon SageMaker Debugger with the following machine learning frameworks and algorithms: TensorFlow, Apache MXNet, PyTorch, and XGBoost\.

**Topics**
+ [Debugger in TensorFlow](#debugger-zero-script-change-TensorFlow)
+ [Debugger in MXNet](#debugger-zero-script-change-MXNet)
+ [Debugger in PyTorch](#debugger-zero-script-change-PyTorch)
+ [Debugger in XGBoost](#debugger-zero-script-change-XGBoost)
+ [Debugger in AWS Containers with Script Mode](#debugger-script-mode)

## Debugger in TensorFlow<a name="debugger-zero-script-change-TensorFlow"></a>

The following code example shows how to enable Debugger in TensorFlow 2\.1\.0\.

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import Rule, DebuggerHookConfig, CollectionConfig, rule_configs

# set path to your Amazon s3 bucket
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-demo-tf2-keras-tensors'
s3_bucket_for_tensors = 's3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'
                        .format(BUCKET_NAME=BUCKET_NAME,
                                LOCATION_IN_BUCKET=LOCATION_IN_BUCKET)

# configure a SageMaker estimator in TF 2.1.0
estimator = TensorFlow(
    role=sagemaker.get_execution_role(),
    base_job_name='smdebug-demo-tf2-keras',
    train_instance_count=1,
    train_instance_type='ml.p2.xlarge',
    entry_point='smdebug-demo-tf2-keras-train.py',
    framework_version='2.1.0',
    py_version='py3',
    train_max_run=3600,
    
    ## Debugger parameters
    rules=[ Rule.sagemaker( ... ) ],
    debugger_hook_config=DebuggerHookConfig(
                      s3_output_path=s3_bucket_for_tensors,  # Required
                      collection_configs=[ ... ]
                        ))

# After calling fit, SageMaker starts
# the training job in the TF 2.1.0 framework.
estimator.fit(wait=True)
```

In the sample code, the lines in bold text activate the pre\-built container in the TF 2\.1\.0 framework with your original training script\. This example assumes the training script is located at `'smdebug-demo-tf2-keras-train.py'`\. 

For more examples using Debugger in TensorFlow containers, see the following example notebook:
+ [Debugger in an AWS Deep Learning Container with the TensorFlow 2\.1 framework](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow2/tensorflow2_zero_code_change/tf2-keras-default-container.ipynb)

## Debugger in MXNet<a name="debugger-zero-script-change-MXNet"></a>

The following code example shows how to enable Debugger in MXNet 1\.6\.0\.

```
import sagemaker
from sagemaker.mxnet import MXNet
from sagemaker.debugger import Rule, DebuggerHookConfig, CollectionConfig, rule_configs

# set path to your Amazon s3 bucket
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-demo-mxnet-tensors'
s3_bucket_for_tensors = 's3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'
                        .format(BUCKET_NAME=BUCKET_NAME,
                                LOCATION_IN_BUCKET=LOCATION_IN_BUCKET)

# configure a SageMaker estimator in MXNet 1.6.0
estimator = MXNet(
    role=sagemaker.get_execution_role(),
    base_job_name='smdebug-demo-mxnet',
    train_instance_count=1,
    train_instance_type='ml.m4.xlarge',
    entry_point='smdebug-demo-mxnet_train.py',
    framework_version='1.6.0',
    train_max_run=3600,
    sagemaker_session=sagemaker.Session(),
    py_version='py3',
    
    ## Debugger parameters
    rules=[ Rule.sagemaker( ... ) ],
    debugger_hook_config=DebuggerHookConfig(
                      s3_output_path=s3_bucket_for_tensors,  # Required
                      collection_configs=[ ... ]
                  ))

# After calling fit, SageMaker starts
# the training job in the MXNet 1.6.0 framework.
estimator.fit(wait=True)
```

For more examples using Debugger in MXNet containers, see the following example notebooks:
+ [ Tensor analysis using Amazon SageMaker Debugger in a pre\-built MXNet Container ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_analysis)
+ [ Visualizing and debugging tensors using Amazon SageMaker Debugger in a pre\-built MXNet container ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mnist_tensor_plot)
+ [ Debugging training jobs in real time with Amazon SageMaker Debugger in a pre\-built MXNet container ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_realtime_analysis)

## Debugger in PyTorch<a name="debugger-zero-script-change-PyTorch"></a>

The following code example shows how to enable Debugger in PyTorch 1\.5\.0\.

```
import sagemaker
from sagemaker.pytorch import PyTorch
from sagemaker.debugger import Rule, DebuggerHookConfig, CollectionConfig, rule_configs

# set path to your Amazon s3 bucket
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-demo-pytorch-tensors'
s3_bucket_for_tensors = 's3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'
                        .format(BUCKET_NAME=BUCKET_NAME,
                                LOCATION_IN_BUCKET=LOCATION_IN_BUCKET)

# configure a SageMaker estimator in PyTorch 1.5.0
estimator = PyTorch(
    role=sagemaker.get_execution_role(),
    base_job_name='smdebug-demo-pytorch',
    train_instance_count=1,
    train_instance_type='ml.p2.xlarge',
    entry_point='smdebug-demo-pytorch-train.py',
    framework_version='1.5.0',
    py_version='py3',
    train_max_run=3600,
    
    ## Debugger parameters
    rules=[ Rule.sagemaker( ... ) ],
    debugger_hook_config=DebuggerHookConfig(
                      s3_output_path=s3_bucket_for_tensors,  # Required
                      collection_configs=[ ... ]
                      ))

# After calling fit, SageMaker starts
# the training job in the PT 1.5 framework.
estimator.fit(wait=True)
```

For more examples using Debugger in PyTorch containers, see the following example notebook:
+ [ Using Amazon SageMaker Debugger and Experiments for iterative model pruning in PyTorch containers ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_iterative_model_pruning)

## Debugger in XGBoost<a name="debugger-zero-script-change-XGBoost"></a>

The XGBoost algorithm can be used as a built\-in algorithm or as a framework like MXNet, PyTorch, or Tensorflow\. If SageMaker XGBoost is used as a built\-in algorithm in container version 0\.90\-2 or later, Debugger is available by default \(zero code change experience\)\.

```
import boto3
import sagemaker
from sagemaker.amazon.amazon_estimator import get_image_uri
from sagemaker.debugger import Rule, DebuggerHookConfig, CollectionConfig, rule_configs
from sagemaker.estimator import Estimator

# set path to your Amazon s3 bucket
BUCKET_NAME = sagemaker_session.default_bucket()
LOCATION_IN_BUCKET = 'smdebug-demo-xgboost-tensors'
s3_bucket_for_tensors = 's3://{BUCKET_NAME}/{LOCATION_IN_BUCKET}'
                        .format(BUCKET_NAME=BUCKET_NAME,
                                LOCATION_IN_BUCKET=LOCATION_IN_BUCKET)

# change the region to be one where your notebook instance is running
region = boto3.Session().region_name

# get the image URI for a XGBoost container
container = get_image_uri(boto3.Session().region_name,
                          'xgboost',
                          repo_version='0.90-2')

# configure a SageMaker estimator in XGBoost 0.90-2
estimator = sagemaker.estimator.Estimator(
    role=sagemaker.get_execution_role(),
    base_job_name="demo-smdebug-xgboost",
    train_instance_count=1,
    train_instance_type='ml.m5.xlarge',
    image_name=container,
    train_max_run=1800,
    
    ## Debugger parameters
    rules=[ Rule.sagemaker( ... ) ],
    debugger_hook_config=DebuggerHookConfig(
                      s3_output_path=s3_bucket_for_tensors,  # Required
                      collection_configs=[ ... ]
                      ))

# After calling fit, SageMaker starts
# the training job in the XGBoost 0.90-2 framework.
estimator.fit({'train':'s3://my-bucket/training',
               'validation':'s3://my-bucket/validation}))
```

For more examples using Debugger in XGBoost containers, see the following example notebooks:
+ [ Debugging XGBoost Training Jobs with Amazon SageMaker Debugger Using Rules ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules)
+ [ Debugging XGBoost training jobs in real time with Amazon SageMaker Debugger ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_realtime_analysis)

To learn more about variations of Debugger configuration, see the [ Debugger configuration guide](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/sagemaker.md#configuring-sagemaker-debugger)\.

To learn more about the Debugger hook and rule features, see [Configure and Save Tensor Data Using the Debugger API Operations](debugger-data.md) and [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

## Debugger in AWS Containers with Script Mode<a name="debugger-script-mode"></a>

To use script mode and enable Debugger to run your training script out of the frameworks supported by the pre\-built AWS containers, you need to make changes to your training script\. Script mode enables you to run the modified training scripts inside the AWS containers\.

Debugger APIs and the open source `smdebug` library support a wider range of frameworks\. You can enable script mode by adding a single line in your estimator and Debugger by adding a few lines in your training Python script\. 

 The following example code shows how to enable script mode in Debugger with TensorFlow\.

```
estimator = TensorFlow(
    ...
    framework_version='1.13',
    script_mode=True,
    ## Debugger rule and hook parameters
    rules = rules,
    debugger_hook_config=hook_config
    ...
)
```

 After enabling script mode, you need to make a minimal changes inside the training script\.

```
# Inside your train.py script:

def train(batch_size, epoch, model, hook):
    model.fit(X_train, Y_train,
              batch_size=batch_size,
              epochs=epoch,
              validation_data=(X_valid, Y_valid),
              shuffle=True,

              # smdebug modification: Pass the hook as a Keras callback
              callbacks=[hook])

def main():
    parser = argparse.ArgumentParser(description="Train resnet50 cifar10")

    # hyperparameter settings
    parser.add_argument( ... )

    model = ResNet50(weights=None, input_shape=(32,32,3), classes=10)

    # Add the following smdebug modification line to activate Debugger hook.
    hook = smd.KerasHook.create_from_json_file()

    # Start the training.
    train(args.batch_size, args.epoch, model, hook)

if __name__ == "__main__":
    main()
```

The following example notebooks show how to apply the changes to training scripts in detail:
+ [ Debugger in script mode with the TensorFlow 2\.1 framework](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow2/tensorflow2_keras_custom_container/tf2-keras-custom-container.ipynb)

  To see the difference between using Debugger in a Deep Learning Container and in script mode, open this notebook and put it and [ the previous Debugger in a Deep Learning Container TensorFlow v2\.1 notebook example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow2/tensorflow2_zero_code_change/tf2-keras-default-container.ipynb) side by side\. 

   In script mode, the hook configuration part is removed from the script in which you set the estimator\. Instead, the Debugger hook feature is merged into the training script, [ TensorFlow Keras ResNet training script in script mode](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow2/tensorflow2_keras_custom_container/src/tf_keras_resnet_byoc.py)\. The training script imports the `smdebug` library in the required TensorFlow Keras environment to communicate with the TensorFlow ResNet50 algorithm\. It also manually implements the `smdebug` hook functionality by adding the `callbacks=[hook]` argument inside the `train` function \(in line 49\), and by adding the manual hook configuration \(in line 89\) provided through SageMaker Python SDK\.

  This script mode example runs the training job in the TF 2\.1 framework for direct comparison with the zero script change in the TF 2\.1 example\. The benefit of setting up Debugger in script mode is the flexibility to choose framework versions not covered by AWS Deep Learning Containers\. 
+ [ Using Amazon SageMaker Debugger in a PyTorch Container in Script Mode ](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_custom_container)

  This notebook enables Debugger in script mode in PyTorch v1\.3\.1 framework\. PyTorch v1\.3\.1 is supported by SageMaker containers, and this example shows details of how to modify a training script\. 

  The SageMaker PyTorch estimator is already in script mode by default\. In the notebook, the line to activate `script_mode` is not included in the estimator configuration\.

  This notebook shows detailed steps to change [ an original PyTorch training script](https://github.com/pytorch/examples/blob/master/mnist/main.py) to [ a modified version with Debugger enabled](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/pytorch_custom_container/scripts/pytorch_mnist.py)\. Additionally, this example shows how you can use Debugger built\-in rules to detect training issues such as the vanishing gradients problem, and the Debugger trial features to call and analyze the saved tensors\. 