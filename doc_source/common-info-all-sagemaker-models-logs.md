# Logs for Built\-in Algorithms<a name="common-info-all-sagemaker-models-logs"></a>

Amazon SageMaker algorithms produce Amazon CloudWatch logs, which provide detailed information on the training process\. To see the logs, in the AWS management console, choose **CloudWatch**, choose **Logs**, and then choose the /aws/sagemaker/TrainingJobs **log group**\. Each training job has one log stream per node on which it was trained\. The log stream’s name begins with the value specified in the `TrainingJobName` parameter when the job was created\.

**Note**  
If a job fails and logs do not appear in CloudWatch, it's likely that an error occurred before the start of training\. Reasons include specifying the wrong training image or S3 location\.

The contents of logs vary by algorithms\. However, you can typically find the following information:
+ Confirmation of arguments provided at the beginning of the log
+ Errors that occurred during training
+ Measurement of an algorithm's accuracy or numerical performance
+ Timings for the algorithm and any major stages within the algorithm

## Common Errors<a name="example-errors"></a>

If a training job fails, some details about the failure are provided by the `FailureReason` return value in the training job description, as follows:

```
sage = boto3.client('sagemaker')
sage.describe_training_job(TrainingJobName=job_name)['FailureReason']
```

Others are reported only in the CloudWatch logs\. Common errors include the following:

1. Misspecifying a hyperparameter or specifying a hyperparameter that is invalid for the algorithm\.

   **From the CloudWatch Log**

   ```
   [10/16/2017 23:45:17 ERROR 139623806805824 train.py:48]
   Additional properties are not allowed (u'mini_batch_siz' was
   unexpected)
   ```

1. Specifying an invalid value for a hyperparameter\.

   **FailureReason**

   ```
   AlgorithmError: u'abc' is not valid under any of the given
   schemas\n\nFailed validating u'oneOf' in
   schema[u'properties'][u'feature_dim']:\n    {u'oneOf':
   [{u'pattern': u'^([1-9][0-9]*)$', u'type': u'string'},\n
   {u'minimum': 1, u'type': u'integer'}]}\
   ```

   **FailureReason**

   ```
   [10/16/2017 23:57:17 ERROR 140373086025536 train.py:48] u'abc'
   is not valid under any of the given schemas
   ```

1. Inaccurate protobuf file format\.

   **From the CloudWatch log**

   ```
   [10/17/2017 18:01:04 ERROR 140234860816192 train.py:48] cannot
                      copy sequence with size 785 to array axis with dimension 784
   ```