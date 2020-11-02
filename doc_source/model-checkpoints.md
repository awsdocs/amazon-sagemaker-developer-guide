# Use Checkpoints in Amazon SageMaker<a name="model-checkpoints"></a>

A checkpoint is a snapshot of the state of the model\. They can be used with Managed Spot Training\. If a training job is interrupted, a snapshot can be used to resume from a previously saved point\. This can save training time\. 

Snapshots are saved to an Amazon S3 location you specify\. You can configure the local path to use for snapshots or use the default\. When a training job is interrupted, Amazon SageMaker copies the training data to Amazon S3\. When the training job is restarted, the checkpoint data is copied to the local path\. It can be used to resume at the checkpoint\. 

To enable checkpoints, provide an Amazon S3 location\. You can optionally provide a local path and choose to use a shared folder\. The default local path is `/opt/ml/checkpoints/`\. For more information, see [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\.