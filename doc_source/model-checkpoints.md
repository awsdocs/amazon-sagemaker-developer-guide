# Use Checkpoints in Amazon SageMaker<a name="model-checkpoints"></a>

Use checkpoints in Amazon SageMaker to save the state of machine learning \(ML\) models during training\. Checkpoints are snapshots of the model and can be configured by callback functions of ML frameworks\. You can use the saved checkpoints to restart a training job from the last saved checkpoint\. 

The SageMaker training mechanism uses training containers on Amazon EC2 instances, and the checkpoint files are saved under a local directory of the containers\. SageMaker provides functionality to copy the checkpoints from the local path to Amazon S3\. Using checkpoints, you can do the following:
+ Save your model snapshots under training due to an unexpected interruption to the training job or instance\.
+ Resume training the model in the future from a checkpoint\.
+ Analyze the model at intermediate stages of training\.
+ Use checkpoints with SageMaker managed spot training to save on training costs\.

If you are using checkpoints with the SageMaker managed spot taining, SageMaker manages checkpointing your model training on a spot instance and resuming the training job on the next spot instance\. With SageMaker managed spot training, you can significantly reduce the billable time for training ML models\. For more information, see [Managed Spot Training in Amazon SageMaker](model-managed-spot-training.md)\.

**Topics**
+ [Checkpoints for Frameworks and Algorithms in SageMaker](#model-checkpoints-whats-supported)
+ [Enable Checkpointing](#model-checkpoints-enable)
+ [Browse Checkpoint Files](#model-checkpoints-saved-file)
+ [Resume Training From a Checkpoint](#model-checkpoints-resume)
+ [Considerations for Checkpointing](#model-checkpoints-considerations)

## Checkpoints for Frameworks and Algorithms in SageMaker<a name="model-checkpoints-whats-supported"></a>

Use checkpoints to save snapshots of ML models built on your preferred frameworks within SageMaker\.

**SageMaker frameworks and algorithms that support checkpointing**

SageMaker supports checkpointing for Deep Learning Containers and a subset of built\-in algorithms without requiring training script changes\. SageMaker saves the checkpoints to the default local path `'/opt/ml/checkpoints'` and copies them to Amazon S3\. 
+ Deep Learning Containers: [TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html), [PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html), and [MXNet](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/sagemaker.mxnet.html)
+ Built\-in algorithms: [Image Classification](https://docs.aws.amazon.com/sagemaker/latest/dg/image-classification.html), [Object Detection](https://docs.aws.amazon.com/sagemaker/latest/dg/object-detection.html), [Semantic Segmentation](https://docs.aws.amazon.com/sagemaker/latest/dg/semantic-segmentation.html), and [XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) \(0\.90\-1 or later\)
**Note**  
If you are using the XGBoost algorithm in framework mode \(script mode\), you need to bring an XGBoost training script with checkpointing that's manually configured\. For more information about the XGBoost training methods to save model snapshots, see [Training XGBoost](https://xgboost.readthedocs.io/en/latest/python/python_intro.html#training) in *the XGBoost Python SDK documentation*\.

If a pre\-built algorithm that does not support checkpointing is used in a managed spot training job, SageMaker does not allow a maximum wait time greater than an hour for the job in order to limit wasted training time from interrupts\.

**For custom training containers and other frameworks**

If you are using your own training containers, training scripts, or other frameworks not listed in the previous section, you must properly set up your training script using callbacks or training APIs to save checkpoints to the local path \(`'/opt/ml/checkpoints'`\) and load from the local path in your training script\. SageMaker estimators can sync up with the local path and save the checkpoints to Amazon S3\.

## Enable Checkpointing<a name="model-checkpoints-enable"></a>

After you enable checkpointing, SageMaker saves checkpoints to Amazon S3 and syncs your training job with the checkpoint S3 bucket\.

![\[Architecture diagram of writing checkpoints during training.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/checkpoints_write.png)

The following example shows how to configure checkpoint paths when you construct a SageMaker estimator\. To enable checkpointing, add the `checkpoint_s3_uri` and `checkpoint_local_path` parameters to your estimator\. 

The following example template shows how to create a generic SageMaker estimator and enable checkpointing\. You can use this template for the supported algorithms by specifying the `image_uri` parameter\. To find Docker image URIs for algorithms with checkpointing supported by SageMaker, see [Docker Registry Paths for SageMaker Built\-in Algorithms](sagemaker-algo-docker-registry-paths.md)\. You can also replace `estimator` and `Estimator` with other SageMaker frameworks' estimator parent classes and estimator classes, such as `[TensorFlow](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html#create-an-estimator)`, `[PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#create-an-estimator)`, `[MXNet](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/using_mxnet.html#create-an-estimator)`, and `[XGBoost](https://sagemaker.readthedocs.io/en/stable/frameworks/xgboost/using_xgboost.html#create-an-estimator)`\.

```
import sagemaker
from sagemaker.estimator import Estimator

bucket=sagemaker.Session().default_bucket()
base_job_name="sagemaker-checkpoint-test"
checkpoint_in_bucket="checkpoints"

# The S3 URI to store the checkpoints
checkpoint_s3_bucket="s3://{}/{}/{}".format(bucket, base_job_name, checkpoint_in_bucket)

# The local path where the model will save its checkpoints in the training container
checkpoint_local_path="/opt/ml/checkpoints"

estimator = Estimator(
    ...
    image_uri="<ecr_path>/<algorithm-name>:<tag>" # Specify to use built-in algorithms
    output_path=bucket,
    base_job_name=base_job_name,
    
    # Parameters required to enable checkpointing
    checkpoint_s3_uri=checkpoint_s3_bucket,
    checkpoint_local_path=checkpoint_local_path
)
```

The following two parameters specify paths for checkpointing:
+ `checkpoint_local_path` – Specify the local path where the model saves the checkpoints periodically in a training container\. The default path is set to `'/opt/ml/checkpoints'`\. If you are using other frameworks or bringing your own training container, ensure that your training script's checkpoint configuration specifies the path to `'/opt/ml/checkpoints'`\.
**Note**  
We recommend specifying the local paths as `'/opt/ml/checkpoints'` to be consistent with the default SageMaker checkpoint settings\. If you prefer to specify your own local path, make sure you match the checkpoint saving path in your training script and the `checkpoint_local_path` parameter of the SageMaker estimators\.
+ `checkpoint_s3_uri` – The URI to an S3 bucket where the checkpoints are stored in real time\. 

To find a complete list of SageMaker estimator parameters, see the [Estimator API](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) in the *[Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) documentation*\.

## Browse Checkpoint Files<a name="model-checkpoints-saved-file"></a>

Locate checkpoint files using the SageMaker Python SDK and the Amazon S3 console\.

**To find the checkpoint files programmatically**

To retrieve the S3 bucket URI where the checkpoints are saved, check the following estimator attribute:

```
estimator.checkpoint_s3_uri
```

This returns the Amazon S3 output path for checkpoints configured while requesting the **CreateTrainingJob** request\. To find the saved checkpoint files using the Amazon S3 console, use the following procedure\.

**To find the checkpoint files from the Amazon S3 console**

1. Sign in to the AWS Management Console and open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Training jobs**\.

1. Choose the link to the training job with checkpointing enabled to open **Job settings**\.

1. In the **Job settings** page of the training job, locate the **Checkpoint configuration** section\.  
![\[Checkpoint configuration section in the Job settings page of a training job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/checkpoints_trainingjob.png)

1. Use the link to the S3 bucket to access the checkpoint files\.

## Resume Training From a Checkpoint<a name="model-checkpoints-resume"></a>

To resume a training job from a checkpoint, run a new estimator with the same `checkpoint_s3_uri` that you created in the [Enable Checkpointing](#model-checkpoints-enable) section\. Once the training has resumed, the checkpoints from this S3 bucket are restored to `checkpoint_local_path` in each instance of the new training job\. Ensure that the S3 bucket is in the same Region as that of the current SageMaker session\.

![\[Architecture diagram of syncing checkpoints to resume training.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/checkpoints_resume.png)

## Considerations for Checkpointing<a name="model-checkpoints-considerations"></a>

Consider the following when using checkpoints in SageMaker\.
+ To avoid overwrites in distributed training with multiple instances, you must manually configure the checkpoint file names and paths in your training script\. The high\-level SageMaker checkpoint configuration specifies a single Amazon S3 location without additional suffixes or prefixes to tag checkpoints from multiple instances\.
+ The SageMaker Python SDK does not support high\-level configuration for checkpointing frequency\. To control the checkpointing frequency, modify your training script using frameworks' model save functions or checkpoint callbacks\.