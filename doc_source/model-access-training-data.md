# Access Training Data<a name="model-access-training-data"></a>

When you create a training job, you specify the location of a training dataset and an input mode for accessing the dataset\. For data location, Amazon SageMaker supports Amazon Simple Storage Service \(Amazon S3\), Amazon Elastic File System \(Amazon EFS\), and Amazon FSx for Lustre\. The input modes determine whether to stream data files of the dataset in real time or download the whole dataset at the start of the training job\.

## SageMaker Input Modes and AWS Cloud Storage<a name="model-access-training-data-input-modes"></a>

This section summarizes SageMaker input modes for Amazon S3 and file systems in Amazon EFS and Amazon FSx for Lustre\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-training-input-mode.png)
+ *File mode* presents a file system view of the dataset to the training container\. This is the default input mode if you don't explicitly specify one of the other two options\. If you use file mode, SageMaker downloads the training data from the storage location to a local directory in the Docker container\. Training starts after the full dataset has been downloaded\. In file mode, the training instance must have enough storage space to fit the entire dataset\. File mode download speed depends on the size of dataset, the average size of files, and the number of files\. You can configure the dataset for file mode by providing either an Amazon S3 prefix, manifest file, or augmented manifest file\. You should use an S3 prefix when all your dataset files are located within a common S3 prefix\. File mode is compatible with [SageMaker local mode](https://sagemaker.readthedocs.io/en/stable/overview.html#local-mode) \(starting a SageMaker training container interactively in seconds\)\. For distributed training, you can shard the dataset across multiple instances with the `ShardedByS3Key` option\.
+ *Fast file mode* provides file system access to an Amazon S3 data source while leveraging the performance advantage of pipe mode\. At the start of training, fast file mode identifies the data files but does not download them\. Training can start without waiting for the entire dataset to download\. This means that the training startup takes less time when there are fewer files in the Amazon S3 prefix provided\.

  In contrast to pipe mode, fast file mode works with random access to the data\. However, it works best when data is read sequentially\. Fast file mode doesn't support augmented manifest files\.

  Fast file mode exposes S3 objects using a POSIX\-compliant file system interface, as if the files are available on the local disk of your training instance\. It streams S3 content on demand as your training script consumes data\. This means that your dataset no longer needs to fit into the training instance storage space as a whole, and you don't need to wait for the dataset to be downloaded to the training instance before training starts\. Fast file currently supports S3 prefixes only \(it does not support manifest and augmented manifest\)\. Fast file mode is compatible with SageMaker local mode\.
+ *Pipe mode* streams data directly from an Amazon S3 data source\. Streaming can provide faster start times and better throughput than file mode\.

  When you stream the data directly, you can reduce the size of the Amazon EBS volumes used by the training instance\. Pipe mode needs only enough disk space to store the final model artifacts\.

  It is another streaming mode that is largely replaced by the newer and simpler\-to\-use fast file mode\. In pipe mode, data is pre\-fetched from Amazon S3 at high concurrency and throughput, and streamed into a named pipe, which also known as a First\-In\-First\-Out \(FIFO\) pipe for its behavior\. Each pipe may only be read by a single process\. A SageMaker specific extension to TensorFlow conveniently [integrates Pipe mode into the native TensorFlow data loader](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html#training-with-pipe-mode-using-pipemodedataset) for streaming text, TFRecords, or RecordIO file formats\. Pipe mode also supports managed sharding and shuffling of data\.
+ Amazon FSx for Lustre – FSx for Lustre can scale to hundreds of gigabytes of throughput and millions of IOPS with low\-latency file retrieval\. When starting a training job, SageMaker mounts the FSx for Lustre file system to the training instance file system, then starts your training script\. Mounting itself is a relatively fast operation that doesn't depend on the size of the dataset stored in FSx for Lustre\. 

  To access FSx for Lustre, your training job must connect to an Amazon Virtual Private Cloud \(VPC\), which requires DevOps setup and involvement\. To avoid data transfer costs, the file system uses a single Availability Zone, and you need to specify a VPC subnet which maps to this Availability Zone ID when running the training job\.
+ Amazon EFS – To use Amazon EFS as a data source, the data must already reside in Amazon EFS prior to training\. SageMaker mounts the specified Amazon EFS file system to the training instance, then starts your training script\. Your training job must connect to a VPC to access Amazon EFS\.
**Tip**  
To learn more about how to specify your VPC configuration to SageMaker estimators, see [Use File Systems as Training Inputs](https://sagemaker.readthedocs.io/en/stable/overview.html?highlight=VPC#use-file-systems-as-training-inputs) in the *SageMaker Python SDK documentation*\.

## Choosing Data Input Mode Using the SageMaker Python SDK<a name="model-access-training-data-using-pysdk"></a>

SageMaker Python SDK provides the generic [Estimator class](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) and its [variations for ML frameworks](https://sagemaker.readthedocs.io/en/stable/frameworks/index.html) for launching training jobs\. You can specify one of the data input modes while configuring the SageMaker `Estimator` class or the `Estimator.fit` method\. The following code templates show the two ways to specify input modes\.

**To specify the input mode using the Estimator class**

```
from sagemaker.estimator import Estimator
from sagemaker.inputs import TrainingInput

estimator = Estimator(
    checkpoint_s3_uri='s3://my-bucket/checkpoint-destination/',
    output_path='s3://my-bucket/output-path/',
    base_job_name='job-name',
    input_mode='File'  # Available options: File | Pipe | FastFile
    ...
)

# Run the training job
estimator.fit(
    inputs=TrainingInput(s3_data="s3://my-bucket/my-data/train")
)
```

For more information, see the [sagemaker\.estimator\.Estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator) class in the *SageMaker Python SDK documentation*\.

**To specify the input mode through the Estimator fit method**

```
from sagemaker.estimator import Estimator
from sagemaker.inputs import TrainingInput

estimator = Estimator(
    checkpoint_s3_uri='s3://my-bucket/checkpoint-destination/',
    output_path='s3://my-bucket/output-path/',
    base_job_name='job-name',
    ...
)

# Run the training job
estimator.fit(
    inputs=TrainingInput(
        s3_data="s3://my-bucket/my-data/train",
        input_mode='File'  # Available options: File | Pipe | FastFile
    )
)
```

For more information, see the [sagemaker\.estimator\.Estimator\.fit](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html#sagemaker.estimator.Estimator.fit) class method and the [sagemaker\.inputs\.TrainingInput](https://sagemaker.readthedocs.io/en/stable/api/utility/inputs.html#sagemaker.inputs.TrainingInput) class in the *SageMaker Python SDK documentation*\.

**Tip**  
To learn more about how to configure Amazon FSx for Lustre or Amazon EFS with your VPC configuration using the SageMaker Python SDK estimators, see [Use File Systems as Training Inputs](https://sagemaker.readthedocs.io/en/stable/overview.html?highlight=VPC#use-file-systems-as-training-inputs) in the *SageMaker Python SDK documentation*\.

**Tip**  
The data input mode integrations with Amazon S3, Amazon EFS, and FSx for Lustre are recommended ways to optimally configure data source for the best practices\. You can strategically improve data loading performance using the SageMaker managed storage options and input modes, but it's not strictly constrained\. You can write your own data reading logic directly in your training container\. For example, you can set to read from a different data source, write your own S3 data loader class, or use third\-party frameworks' data loading functions within your training script\. However, you must make sure that you specify the right paths that SageMaker can recognize\.

**Tip**  
If you use a custom training container, make sure you install the [SageMaker training toolkit](https://github.com/aws/sagemaker-training-toolkit) that helps set up the environment for SageMaker training jobs\. Otherwise, you must specify the environment variables explicitly in your Dockerfile\. For more information, see [Create a container with your own algorithms and models](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers-create.html)\.

For more information about how to set the data input modes using the low\-level SageMaker APIs, see [How Amazon SageMaker Provides Training Information](your-algorithms-training-algo-running-container.md), the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API, and the `TrainingInputMode` in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html)\.

## Best Practices for Choosing Data Source and Input Mode<a name="model-access-training-data-best-practices"></a>

The best data source for your training job depends on workload characteristics such as the size of the dataset, the file format, the average size of files, the training duration, a sequential or random data loader read pattern, and how fast your model can consume the training data\. The following best practices provide guidelines to get started with the most suitable input mode and data storage for your use case\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-training-choose-mode-and-storage.png)

### When to use Amazon EFS<a name="model-access-training-data-best-practices-efs"></a>

If your dataset is stored in Amazon Elastic File System, you might have a preprocessing or annotations application that uses Amazon EFS for storage\. You can run a training job configured with a data channel that points to the Amazon EFS file system\. For more information, see [Speed up training on Amazon SageMaker using Amazon FSx for Lustre and Amazon EFS file systems](https://aws.amazon.com/blogs/machine-learning/speed-up-training-on-amazon-sagemaker-using-amazon-efs-or-amazon-fsx-for-lustre-file-systems/)\. If you cannot achieve better performace, check your optimization options following the [Amazon Elastic File System performance guide](https://docs.aws.amazon.com/efs/latest/ug/performance.html#performance-overview) or consider using different input modes or data storage\.

### Use file mode for small datasets<a name="model-access-training-data-best-practices-file-mode"></a>

If the dataset is stored in Amazon Simple Storage Service and its overall volume is relatively small \(for example, less than 50\-100 GB\), try using file mode\. The overhead of downloading a 50 GB dataset can vary based on the total number of files\. For example, it takes about 5 minutes if a dataset is chunked into 100 MB shards\. Whether this startup overhead is acceptable primarily depends on the overall duration of your training job, because a longer training phase means a proportionally smaller download phase\.

### Serializing many small files<a name="model-access-training-data-best-practices-serialize"></a>

If your dataset size is small \(less than 50\-100 GB\), but is made up of many small files \(less than 50 MB per file\), the file mode download overhead grows, because each file needs to be downloaded individually from Amazon Simple Storage Service to the training instance volume\. To reduce this overhead and data traversal time in general, consider serializing groups of such small files into fewer larger file containers \(such as 150 MB per file\) by using file formats, such as [TFRecord](https://www.tensorflow.org/tutorials/load_data/tfrecord) for TensorFlow, [ WebDataset](https://webdataset.github.io/webdataset/) for PyTorch, and [RecordIO](https://mxnet.apache.org/versions/1.8.0/api/faq/recordio) for MXNet\.

### When to use fast file mode<a name="model-access-training-data-best-practices-fastfile"></a>

For larger datasets with larger files \(more than 50 MB per file\), the first option is to try fast file mode, which is more straightforward to use than FSx for Lustre because it doesn't require creating a file system, or connecting to a VPC\. Fast file mode is ideal for large file containers \(more than 150 MB\), and might also do well with files more than 50 MB\. Because fast file mode provides a POSIX interface, it supports random reads \(reading non\-sequential byte\-ranges\)\. However, this is not the ideal use case, and your throughput might be lower than with the sequential reads\. However, if you have a relatively large and computationally intensive ML model, fast file mode might still be able to saturate the effective bandwidth of the training pipeline and not result in an IO bottleneck\. You'll need to experiment and see\. To switch from file mode to fast file mode \(and back\), just add \(or remove\) the `input_mode='FastFile'` parameter while defining your input channel using the SageMaker Python SDK:

```
sagemaker.inputs.TrainingInput(S3_INPUT_FOLDER,  input_mode = 'FastFile')
```

### When to use Amazon FSx for Lustre<a name="model-access-training-data-best-practices-fsx"></a>

If your dataset is too large for file mode, has many small files that you can't serialize easily, or uses a random read access pattern, FSx for Lustre is a good option to consider\. Its file system scales to hundreds of gigabytes per second \(GB/s\) of throughput and millions of IOPS, which is ideal when you have many small files\. However, note that there might be the cold start issue due to lazy loading and the overhead of setting up and initializing the FSx for Lustre file system\.

**Tip**  
To learn more, see [Choose the best data source for your Amazon SageMaker training job](http://aws.amazon.com/blogs/machine-learning/choose-the-best-data-source-for-your-amazon-sagemaker-training-job/)\. This AWS machine learning blog further discusses case studies and performance benchmark of data sources and input modes\.