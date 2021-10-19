# How Amazon SageMaker Processes Training Output<a name="your-algorithms-training-algo-output"></a>

As your algorithm runs in a container, it generates output including the status of the training job and model and output artifacts\. Your algorithm should write this information to the following files, which are located in the container's `/output` directory\. Amazon SageMaker processes the information contained in this directory as follows:
+ `/opt/ml/output/failure`—If training fails, after all algorithm output \(for example, logging\) completes, your algorithm should write the failure description to this file\. In a `DescribeTrainingJob` response, SageMaker returns the first 1024 characters from this file as `FailureReason`\. 

   
+ `/opt/ml/model`—Your algorithm should write all final model artifacts to this directory\. SageMaker copies this data as a single object in compressed tar format to the S3 location that you specified in the `CreateTrainingJob` request\.  If multiple containers in a single training job write to this directory they should ensure no `file/directory` names clash\. SageMaker aggregates the result in a tar file and uploads to s3\. 