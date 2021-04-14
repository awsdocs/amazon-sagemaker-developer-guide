# Visualize Amazon SageMaker Debugger Output Tensors in TensorBoard<a name="debugger-enable-tensorboard-summaries"></a>

Use SageMaker Debugger to create output tensor files that are compatible with TensorBoard\. Load the files to visualize in TensorBoard and analyze your SageMaker training jobs\. Debugger automatically generates output tensor files that are compatible with TensorBoard\. For any hook configuration you customize for saving output tensors, Debugger has the flexibility to create scalar summaries, distributions, and histograms that you can import to TensorBoard\. 

![\[An architecture diagram of the Debugger output tensor saving mechanism.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-tensorboard-concept.png)

You can enable this by passing `DebuggerHookConfig` and `TensorBoardOutputConfig` objects to an `estimator`\.

The following procedure explains how to save scalars, weights, and biases as full tensors, histograms, and distributions that can be visualized with TensorBoard\. Debugger saves them to the training container's local path \(the default path is `/opt/ml/output/tensors`\) and syncs to the Amazon S3 locations passed through the Debugger output configuration objects\.

**To save TensorBoard compatible output tensor files using Debugger**

1. Set up a `tensorboard_output_config` configuration object to save TensorBoard output using the Debugger `TensorBoardOutputConfig` class\. For the `s3_output_path` parameter, specify the default S3 bucket of the current SageMaker session or a preferred S3 bucket\. This example does not add the `container_local_output_path` parameter; instead, it is set to the default local path `/opt/ml/output/tensors`\.

   ```
   import sagemaker
   from sagemaker.debugger import TensorBoardOutputConfig
   
   bucket = sagemaker.Session().default_bucket()
   tensorboard_output_config = TensorBoardOutputConfig(
       s3_output_path='s3://{}'.format(bucket)
   )
   ```

   For additional information, see the Debugger `[TensorBoardOutputConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.TensorBoardOutputConfig)` API in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

1. Configure the Debugger hook and customize the hook parameter values\. For example, the following code configures a Debugger hook to save all scalar outputs every 100 steps in training phases and 10 steps in validation phases, the `weights` parameters every 500 steps \(the default `save_interval` value for saving tensor collections is 500\), and the `bias` parameters every 10 global steps until the global step reaches 500\.

   ```
   from sagemaker.debugger import CollectionConfig, DebuggerHookConfig
   
   hook_config = DebuggerHookConfig(
       hook_parameters={
           "train.save_interval": "100",
           "eval.save_interval": "10"
       },
       collection_configs=[
           CollectionConfig("weights"),
           CollectionConfig(
               name="biases",
               parameters={
                   "save_interval": "10",
                   "end_step": "500",
                   "save_histogram": "True"
               }
           ),
       ]
   )
   ```

   For more information about the Debugger configuration APIs, see the Debugger `[CollectionConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig)` and `[DebuggerHookConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.DebuggerHookConfig)` APIs in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

1. Construct a SageMaker estimator with the Debugger parameters passing the configuration objects\. The following example template shows how to create a generic SageMaker estimator\. You can replace `estimator` and `Estimator` with other SageMaker frameworks' estimator parent classes and estimator classes\. Available SageMaker framework estimators for this functionality are `[TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html#create-an-estimator)`, `[PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#create-an-estimator)`, and `[MXNet](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/using_mxnet.html#create-an-estimator)`\.

   ```
   from sagemaker.estimator import Estimator
   
   estimator = Estimator(
       ...
       # Debugger parameters
       debugger_hook_config=hook_config,
       tensorboard_output_config=tensorboard_output_config
   )
   estimator.fit()
   ```

   The `estimator.fit()` method starts a training job, and Debugger writes the output tensor files in real time to the Debugger S3 output path and to the TensorBoard S3 output path\. To retrieve the output paths, use the following estimator methods:
   + For the Debugger S3 output path, use `estimator.latest_job_debugger_artifacts_path()`\.
   + For the TensorBoard S3 output path, use `estimator.latest_job_tensorboard_artifacts_path()`\.

1. After the training has completed, check the names of saved output tensors:

   ```
   from smdebug.trials import create_trial
   trial = create_trial(estimator.latest_job_debugger_artifacts_path())
   trial.tensor_names()
   ```

1. Check the TensorBoard output data in Amazon S3:

   ```
   tensorboard_output_path=estimator.latest_job_tensorboard_artifacts_path()
   print(tensorboard_output_path)
   !aws s3 ls {tensorboard_output_path}/
   ```

1. Download the TensorBoard output data to your notebook instance\. For example, the following AWS CLI command downloads the TensorBoard files to `/logs/fit` under the current working directory of your notebook instance\.

   ```
   !aws s3 cp --recursive {tensorboard_output_path} ./logs/fit
   ```

1. Compress the file directory to a TAR file to download to your local machine\.

   ```
   !tar -cf logs.tar logs
   ```

1. Download and extract the Tensorboard TAR file to a directory on your device, launch a Jupyter notebook server, open a new notebook, and run the TensorBoard app\.

   ```
   !tar -xf logs.tar
   %load_ext tensorboard
   %tensorboard --logdir logs/fit
   ```

The following animated screenshot illustrates steps 5 through 8\. It demonstrates how to download the Debugger TensorBoard TAR file and load the file in a Jupyter notebook on your local device\.

![\[An animated screenshot showing how to download and load the Debugger TensorBoard file on your local machine.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-tensorboard.gif)