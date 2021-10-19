# TrainingJob operator<a name="trainingjob-operator"></a>

Training job operators reconcile your specified training job spec to SageMaker by launching it for you in SageMaker\. You can learn more about SageMaker training jobs in the SageMaker [CreateTrainingJob API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTrainingJob.html)\. 

**Topics**
+ [Create a TrainingJob Using a YAML File](#create-a-trainingjob-using-a-simple-yaml-file)
+ [Create a TrainingJob Using a Helm Chart](#create-a-trainingjob-using-a-helm-chart)
+ [List TrainingJobs](#list-training-jobs)
+ [Describe a TrainingJob](#describe-a-training-job)
+ [View Logs from TrainingJobs](#view-logs-from-training-jobs)
+ [Delete TrainingJobs](#delete-training-jobs)

## Create a TrainingJob Using a YAML File<a name="create-a-trainingjob-using-a-simple-yaml-file"></a>

1. Download the sample YAML file for training using the following command: 

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/xgboost-mnist-trainingjob.yaml
   ```

1. Edit the `xgboost-mnist-trainingjob.yaml` file to replace the `roleArn` parameter with your `<sagemaker-execution-role>`, and `outputPath` with your Amazon S3 bucket that the SageMaker execution role has write access to\. The `roleArn` must have permissions so that SageMaker can access Amazon S3, Amazon CloudWatch, and other services on your behalf\. For more information on creating an SageMaker ExecutionRole, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createtrainingjob-perms)\. Apply the YAML file using the following command: 

   ```
   kubectl apply -f xgboost-mnist-trainingjob.yaml
   ```

## Create a TrainingJob Using a Helm Chart<a name="create-a-trainingjob-using-a-helm-chart"></a>

You can use Helm Charts to run TrainingJobs\. 

1. Clone the GitHub repository to get the source using the following command: 

   ```
   git clone https://github.com/aws/amazon-sagemaker-operator-for-k8s.git
   ```

1. Navigate to the `amazon-sagemaker-operator-for-k8s/hack/charts/training-jobs/` folder and edit the `values.yaml` file to replace values like `rolearn` and `outputpath` with values that correspond to your account\. The RoleARN must have permissions so that SageMaker can access Amazon S3, Amazon CloudWatch, and other services on your behalf\. For more information on creating an SageMaker ExecutionRole, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createtrainingjob-perms)\. 

### Create the TrainingJob<a name="create-the-training-job"></a>

With the roles and Amazon S3 buckets replaced with appropriate values in `values.yaml`, you can create a training job using the following command: 

```
helm install . --generate-name
```

Your output should look like the following: 

```
NAME: chart-12345678
LAST DEPLOYED: Wed Nov 20 23:35:49 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thanks for installing the sagemaker-k8s-trainingjob.
```

### Verify Your Training Helm Chart<a name="verify-your-training-helm-chart"></a>

To verify that the Helm Chart was created successfully, run: 

```
helm ls
```

Your output should look like the following: 

```
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
chart-12345678        default         1               2019-11-20 23:35:49.9136092 +0000 UTC   deployed        sagemaker-k8s-trainingjob-0.1.0
rolebased-12345678    default         1               2019-11-20 23:14:59.6777082 +0000 UTC   deployed        sagemaker-k8s-operator-0.1.0
```

`helm install` creates a `TrainingJob` Kubernetes resource\. The operator launches the actual training job in SageMaker and updates the `TrainingJob` Kubernetes resource to reflect the status of the job in SageMaker\. You incur charges for SageMaker resources used during the duration of your job\. You do not incur any charges once your job completes or stops\. 

**Note**: SageMaker does not allow you to update a running training job\. You cannot edit any parameter and re\-apply the file/config\. Either change the metadata name or delete the existing job and create a new one\. Similar to existing training job operators like TFJob in Kubeflow, `update` is not supported\. 

## List TrainingJobs<a name="list-training-jobs"></a>

Use the following command to list all jobs created using the Kubernetes operator: 

```
kubectl get TrainingJob
```

The output listing all jobs should look like the following: 

```
kubectl get trainingjobs
NAME                        STATUS       SECONDARY-STATUS   CREATION-TIME          SAGEMAKER-JOB-NAME
xgboost-mnist-from-for-s3   InProgress   Starting           2019-11-20T23:42:35Z   xgboost-mnist-from-for-s3-examplef11eab94e0ed4671d5a8f
```

A training job continues to be listed after the job has completed or failed\. You can remove a `TrainingJob` job from the list by following the Delete a Training Job steps\. Jobs that have completed or stopped do not incur any charges for SageMaker resources\. 

### TrainingJob Status Values<a name="training-job-status-values"></a>

The `STATUS` field can be one of the following values: 
+ `Completed` 
+ `InProgress` 
+ `Failed` 
+ `Stopped` 
+ `Stopping` 

These statuses come directly from the SageMaker official [API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeTrainingJob.html#SageMaker-DescribeTrainingJob-response-TrainingJobStatus)\. 

In addition to the official SageMaker status, it is possible for `STATUS` to be `SynchronizingK8sJobWithSageMaker`\. This means that the operator has not yet processed the job\. 

### Secondary Status Values<a name="secondary-status-values"></a>

The secondary statuses come directly from the SageMaker official [API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeTrainingJob.html#SageMaker-DescribeTrainingJob-response-SecondaryStatus)\. They contain more granular information about the status of the job\. 

## Describe a TrainingJob<a name="describe-a-training-job"></a>

You can get more details about the training job by using the `describe` `kubectl` command\. This is typically used for debugging a problem or checking the parameters of a training job\. To get information about your training job, use the following command: 

```
kubectl describe trainingjob xgboost-mnist-from-for-s3
```

The output for your training job should look like the following: 

```
Name:         xgboost-mnist-from-for-s3
Namespace:    default
Labels:       <none>
Annotations:  <none>
API Version:  sagemaker.aws.amazon.com/v1
Kind:         TrainingJob
Metadata:
  Creation Timestamp:  2019-11-20T23:42:35Z
  Finalizers:
    sagemaker-operator-finalizer
  Generation:        2
  Resource Version:  23119
  Self Link:         /apis/sagemaker.aws.amazon.com/v1/namespaces/default/trainingjobs/xgboost-mnist-from-for-s3
  UID:               6d7uiui-0bef-11ea-b94e-0ed467example
Spec:
  Algorithm Specification:
    Training Image:       8256416981234.dkr.ecr.us-east-2.amazonaws.com/xgboost:1
    Training Input Mode:  File
  Hyper Parameters:
    Name:   eta
    Value:  0.2
    Name:   gamma
    Value:  4
    Name:   max_depth
    Value:  5
    Name:   min_child_weight
    Value:  6
    Name:   num_class
    Value:  10
    Name:   num_round
    Value:  10
    Name:   objective
    Value:  multi:softmax
    Name:   silent
    Value:  0
  Input Data Config:
    Channel Name:      train
    Compression Type:  None
    Content Type:      text/csv
    Data Source:
      S 3 Data Source:
        S 3 Data Distribution Type:  FullyReplicated
        S 3 Data Type:               S3Prefix
        S 3 Uri:                     https://s3-us-east-2.amazonaws.com/my-bucket/sagemaker/xgboost-mnist/train/
    Channel Name:                    validation
    Compression Type:                None
    Content Type:                    text/csv
    Data Source:
      S 3 Data Source:
        S 3 Data Distribution Type:  FullyReplicated
        S 3 Data Type:               S3Prefix
        S 3 Uri:                     https://s3-us-east-2.amazonaws.com/my-bucket/sagemaker/xgboost-mnist/validation/
  Output Data Config:
    S 3 Output Path:  s3://my-bucket/sagemaker/xgboost-mnist/xgboost/
  Region:             us-east-2
  Resource Config:
    Instance Count:     1
    Instance Type:      ml.m4.xlarge
    Volume Size In GB:  5
  Role Arn:             arn:aws:iam::12345678910:role/service-role/AmazonSageMaker-ExecutionRole
  Stopping Condition:
    Max Runtime In Seconds:  86400
  Training Job Name:         xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94e0example
Status:
  Cloud Watch Log URL:           https://us-east-2.console.aws.amazon.com/cloudwatch/home?region=us-east-2#logStream:group=/aws/sagemaker/TrainingJobs;prefix=<example>;streamFilter=typeLogStreamPrefix
  Last Check Time:               2019-11-20T23:44:29Z
  Sage Maker Training Job Name:  xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94eexample
  Secondary Status:              Downloading
  Training Job Status:           InProgress
Events:                          <none>
```

## View Logs from TrainingJobs<a name="view-logs-from-training-jobs"></a>

Use the following command to see the logs from the `kmeans-mnist` training job: 

```
kubectl smlogs trainingjob xgboost-mnist-from-for-s3
```

Your output should look similar to the following\. The logs from instances are ordered chronologically\. 

```
"xgboost-mnist-from-for-s3" has SageMaker TrainingJobName "xgboost-mnist-from-for-s3-123456789" in region "us-east-2", status "InProgress" and secondary status "Starting"
xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94e0ed46example/algo-1-1574293123 2019-11-20 23:45:24.7 +0000 UTC Arguments: train
xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94e0ed46example/algo-1-1574293123 2019-11-20 23:45:24.7 +0000 UTC [2019-11-20:23:45:22:INFO] Running standalone xgboost training.
xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94e0ed46example/algo-1-1574293123 2019-11-20 23:45:24.7 +0000 UTC [2019-11-20:23:45:22:INFO] File size need to be processed in the node: 1122.95mb. Available memory size in the node: 8586.0mb
xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94e0ed46example/algo-1-1574293123 2019-11-20 23:45:24.7 +0000 UTC [2019-11-20:23:45:22:INFO] Determined delimiter of CSV input is ','
xgboost-mnist-from-for-s3-6d7fa0af0bef11eab94e0ed46example/algo-1-1574293123 2019-11-20 23:45:24.7 +0000 UTC [23:45:22] S3DistributionType set as FullyReplicated
```

## Delete TrainingJobs<a name="delete-training-jobs"></a>

Use the following command to stop a training job on Amazon SageMaker: 

```
kubectl delete trainingjob xgboost-mnist-from-for-s3
```

This command removes the SageMaker training job from Kubernetes\. This command returns the following output: 

```
trainingjob.sagemaker.aws.amazon.com "xgboost-mnist-from-for-s3" deleted
```

If the job is still in progress on SageMaker, the job stops\. You do not incur any charges for SageMaker resources after your job stops or completes\. 

**Note**: SageMaker does not delete training jobs\. Stopped jobs continue to show on the SageMaker console\. The `delete` command takes about 2 minutes to clean up the resources from SageMaker\. 