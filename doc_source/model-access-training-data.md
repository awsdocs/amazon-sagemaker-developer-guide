# Access Training Data<a name="model-access-training-data"></a>

When you create a training job, you specify the location of the training dataset and the input mode for accessing that data\. For data location, Amazon SageMaker supports Amazon Simple Storage Service \(Amazon S3\), Amazon Elastic File System \(Amazon EFS\), and Amazon FSx for Lustre, dependent on the input mode\. The input mode determines whether the data is streamed or downloaded at the start of the training job\.

**Input modes**
+ **File** mode presents a file system view of the dataset to the training container\. Data sources can be Amazon S3 or Amazon EFS and Amazon FSx remote file systems\.

  **File** mode downloads the training data from the storage location to a local directory in the Docker container\. Training starts after the full dataset has been downloaded\.
+ **Pipe** mode streams data directly from an Amazon S3 data source\. Streaming can provide faster start times and better throughput than `File` mode\.

  When you stream the data directly, you can reduce the size of the Amazon EBS volumes used by the training instance\. `Pipe` mode needs only enough disk space to store the final model artifacts\.
+ **FastFile** mode provides file system access to an Amazon S3 data source while leveraging the performance advantage of `Pipe` mode\. At the start of training, `FastFile` mode identifies the data files but does not download them\. Training can start without waiting for the entire dataset to download\. The start\-up time is lower when there are fewer files in the Amazon S3 prefix provided\.

  In contrast to `Pipe` mode, `FastFile` mode works with random access to the data\. However, it works best when data is read sequentially\. `FastFile` mode doesn't support augmented manifest files\.

For more information, see [How Amazon SageMaker Provides Training Information](your-algorithms-training-algo-running-container.md), the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API, and the `TrainingInputMode` in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html)\.