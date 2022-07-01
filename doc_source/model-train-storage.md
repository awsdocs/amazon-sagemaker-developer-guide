# Amazon SageMaker Training Storage Folders for Training Datasets, Checkpoints, Model Artifacts, and Outputs<a name="model-train-storage"></a>

This page provides a high\-level summary of how the SageMaker training platform manages storage paths for training datasets, model artifacts, checkpoints, and outputs between AWS cloud storage and training jobs in SageMaker\. Throughout this guide, you learn to identify the default paths set by the SageMaker platform and how the data channels can be streamlined with your data sources in Amazon S3, FSx for Lustre, and Amazon EFS\. For more information about various data channel input modes and storage options, see [Access Training Data](model-access-training-data.md)\.

**Topics**
+ [Overview](#model-train-storage-overview)
+ [SageMaker Environment Variables and Default Paths for Training Storage Folders](#model-train-storage-env-var-summary)

## Overview<a name="model-train-storage-overview"></a>

The following diagram shows the simplest example of how SageMaker manages input and output folders when you run a training job using the SageMaker Python SDK [Estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) class and its [fit](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator.fit) method\. It's based on using file mode as the data access strategy and Amazon S3 as the data source for the training input channels\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-training-storage.png)

For more information and examples of how SageMaker manages data source, input modes, and local paths in SageMaker training instances, see [Access Training Data](https://docs.aws.amazon.com/sagemaker/latest/dg/model-access-training-data.html)\.

## SageMaker Environment Variables and Default Paths for Training Storage Folders<a name="model-train-storage-env-var-summary"></a>

The following table summarizes input and output paths for training datasets, checkpoints, model artifacts, and outputs, managed by the SageMaker training platform\.


| Local path in SageMaker training instance | SageMaker environment variable | Purpose | Read from S3 during start | Read from S3 during Spot\-restart | Writes to S3 during training | Writes to S3 when job is terminated | 
| --- | --- | --- | --- | --- | --- | --- | 
|  `/opt/ml/input/data/channel_name`1   |  SM\_CHANNEL\_*CHANNEL\_NAME*  |  Reading training data from the input channels specified through the SageMaker Python SDK [Estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) class or the [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. For more information about how to specify it in your training script using the SageMaker Python SDK, see [Prepare a Training script](https://sagemaker.readthedocs.io/en/stable/overview.html?highlight=VPC#prepare-a-training-script)\.  | Yes | Yes | No | No | 
|  `/opt/ml/output/`2  | SM\_OUTPUT\_DIR |  Saving outputs such as loss, accuracy, intermediate layers, weights, gradients, bias, and TensorBoard\-compatible outputs\. You can also save any arbitrary output you’d like using this path\. Note that this is a different path from the one for storing the final model artifact `/opt/ml/model/`\.  | No | No | No | Yes | 
|  `/opt/ml/model/`3  | SM\_MODEL\_DIR |  Storing the final model artifact\. This is also the path from where the model artifact is deployed for [Real\-time inference](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html) in SageMaker Hosting\.  | No | No | No | Yes | 
|  `/opt/ml/checkpoints/`4  | None |  Saving model checkpoints \(the state of model\) to resume training from a certain point, and recover from unexpected or [Managed Spot Training](https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html) interruptions\.  | Yes | Yes | Yes | No | 
|  `/opt/ml/code/`  | SAGEMAKER\_SUBMIT\_DIRECTORY |  Copying training scripts, additional libraries, and dependencies\.  | Yes | Yes | No | No | 

1 `channel_name` is the place to specify user\-defined channel names for training data inputs\. Each training job can contain several data input channels\. You can specify up to 20 training input channels per training job\. Note that the data downloading time from the data channels is counted to the billable time\. For more information about data input paths, see [How Amazon SageMaker Provides Training Information](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo-running-container.html)\. Also, there are three types of data input modes that SageMaker supports: file, FastFile, and pipe mode\. To learn more about the data input modes for training in SageMaker, see [Access Training Data](https://docs.aws.amazon.com/sagemaker/latest/dg/model-access-training-data.html)\.

2 SageMaker compresses and writes training artifacts to TAR files \(`tar.gz`\)\. Compression and uploading time is counted to the billable time\. For more information, see [How Amazon SageMaker Processes Training Output](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo-output.html)\.

3 SageMaker compresses and writes the final model artifact to a TAR file \(`tar.gz`\)\. Compression and uploading time is counted to the billable time\. For more information, see [How Amazon SageMaker Processes Training Output](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-training-algo-output.html)\.

4 Sync with Amazon S3 during training\. Write as is without compressing to TAR files\. For more information, see [Use Checkpoints in Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/model-checkpoints.html)\.

**Note**  
If you want to store training artifacts for distributed training in the `/opt/ml/output/` directory, you must properly assign subfolders or unique file names for the artifacts through your model definition or training script\. If the subfolders and file names are not properly configured, all of the distributed training workers might write outputs to the same file name in the same output folder in Amazon S3\.

**Tip**  
To learn more, see [Choose the best data source for your Amazon SageMaker training job](http://aws.amazon.com/blogs/machine-learning/choose-the-best-data-source-for-your-amazon-sagemaker-training-job/)\. This AWS machine learning blog further discusses case studies and performance benchmark of data sources and input modes\.

**Tip**  
If you use a custom training container, make sure you install the [SageMaker Training Toolkit](https://github.com/aws/sagemaker-training-toolkit) that helps set up the environment for SageMaker training jobs\. Otherwise, you must specify the environment variables explicitly in your Dockerfile\. For more information, see [Create a container with your own algorithms and models](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers-create.html)\.