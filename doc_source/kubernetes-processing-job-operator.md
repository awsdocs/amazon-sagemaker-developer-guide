# ProcessingJob operator<a name="kubernetes-processing-job-operator"></a>

ProcessingJob operators are used to launch Amazon SageMaker processing jobs\. For more information on SageMaker processing jobs, see [CreateProcessingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html)\. 

**Topics**
+ [Create a ProcessingJob Using a YAML File](#kubernetes-processing-job-yaml)
+ [List ProcessingJobs](#kubernetes-processing-job-list)
+ [Describe a ProcessingJob](#kubernetes-processing-job-description)
+ [Delete a ProcessingJob](#kubernetes-processing-job-delete)

## Create a ProcessingJob Using a YAML File<a name="kubernetes-processing-job-yaml"></a>

Follow these steps to create an Amazon SageMaker processing job by using a YAML file:

1. Download the `kmeans_preprocessing.py` pre\-processing script\.

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/kmeans_preprocessing.py
   ```

1. In one of your Amazon Simple Storage Service \(Amazon S3\) buckets, create a `mnist_kmeans_example/processing_code` folder and upload the script to the folder\.

1. Download the `kmeans-mnist-processingjob.yaml` file\.

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/kmeans-mnist-processingjob.yaml
   ```

1. Edit the YAML file to specify your `sagemaker-execution-role` and replace all instances of `my-bucket` with your S3 bucket\.

   ```
   ...
   metadata:
     name: kmeans-mnist-processing
   ...
     roleArn: arn:aws:iam::<acct-id>:role/service-role/<sagemaker-execution-role>
     ...
     processingOutputConfig:
       outputs:
         ...
             s3Output:
               s3Uri: s3://<my-bucket>/mnist_kmeans_example/output/
     ...
     processingInputs:
       ...
           s3Input:
             s3Uri: s3://<my-bucket>/mnist_kmeans_example/processing_code/kmeans_preprocessing.py
   ```

   The `sagemaker-execution-role` must have permissions so that SageMaker can access your S3 bucket, Amazon CloudWatch, and other services on your behalf\. For more information on creating an execution role, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createtrainingjob-perms)\.

1. Apply the YAML file using one of the following commands\.

   For cluster\-scoped installation:

   ```
   kubectl apply -f kmeans-mnist-processingjob.yaml
   ```

   For namespace\-scoped installation:

   ```
   kubectl apply -f kmeans-mnist-processingjob.yaml -n <NAMESPACE>
   ```

## List ProcessingJobs<a name="kubernetes-processing-job-list"></a>

Use one of the following commands to list all the jobs created using the ProcessingJob operator\. `SAGEMAKER-JOB-NAME ` comes from the `metadata` section of the YAML file\.

For cluster\-scoped installation:

```
kubectl get ProcessingJob kmeans-mnist-processing
```

For namespace\-scoped installation:

```
kubectl get ProcessingJob -n <NAMESPACE> kmeans-mnist-processing
```

Your output should look similar to the following:

```
NAME                    STATUS     CREATION-TIME        SAGEMAKER-JOB-NAME
kmeans-mnist-processing InProgress 2020-09-22T21:13:25Z kmeans-mnist-processing-7410ed52fd1811eab19a165ae9f9e385
```

The output lists all jobs regardless of the their status\. To remove a job from the list, see [Delete a Processing Job](https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-processing-job-operator.html#kubernetes-processing-job-delete)\.

**ProcessingJob Status**
+ `SynchronizingK8sJobWithSageMaker` – The job is first submitted to the cluster\. The operator has received the request and is preparing to create the processing job\.
+ `Reconciling` – The operator is initializing or recovering from transient errors, along with others\. If the processing job remains in this state, use the `kubectl` `describe` command to see the reason in the `Additional` field\.
+ `InProgress | Completed | Failed | Stopping | Stopped` – Status of the SageMaker processing job\. For more information, see [DescribeProcessingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html#sagemaker-DescribeProcessingJob-response-ProcessingJobStatus)\.
+ `Error` – The operator cannot recover by reconciling\.

Jobs that have completed, stopped, or failed do not incur further charges for SageMaker resources\.

## Describe a ProcessingJob<a name="kubernetes-processing-job-description"></a>

Use one of the following commands to get more details about a processing job\. These commands are typically used for debugging a problem or checking the parameters of a processing job\.

For cluster\-scoped installation:

```
kubectl describe processingjob kmeans-mnist-processing
```

For namespace\-scoped installation:

```
kubectl describe processingjob kmeans-mnist-processing -n <NAMESPACE>
```

The output for your processing job should look similar to the following\.

```
$ kubectl describe ProcessingJob kmeans-mnist-processing
Name:         kmeans-mnist-processing
Namespace:    default
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"sagemaker.aws.amazon.com/v1","kind":"ProcessingJob","metadata":{"annotations":{},"name":"kmeans-mnist-processing",...
API Version:  sagemaker.aws.amazon.com/v1
Kind:         ProcessingJob
Metadata:
  Creation Timestamp:  2020-09-22T21:13:25Z
  Finalizers:
    sagemaker-operator-finalizer
  Generation:        2
  Resource Version:  21746658
  Self Link:         /apis/sagemaker.aws.amazon.com/v1/namespaces/default/processingjobs/kmeans-mnist-processing
  UID:               7410ed52-fd18-11ea-b19a-165ae9f9e385
Spec:
  App Specification:
    Container Entrypoint:
      python
      /opt/ml/processing/code/kmeans_preprocessing.py
    Image Uri:  763104351884.dkr.ecr.us-west-2.amazonaws.com/pytorch-training:1.5.0-cpu-py36-ubuntu16.04
  Environment:
    Name:   MYVAR
    Value:  my_value
    Name:   MYVAR2
    Value:  my_value2
  Network Config:
  Processing Inputs:
    Input Name:  mnist_tar
    s3Input:
      Local Path:   /opt/ml/processing/input
      s3DataType:   S3Prefix
      s3InputMode:  File
      s3Uri:        s3://<s3bucket>-us-west-2/algorithms/kmeans/mnist/mnist.pkl.gz
    Input Name:     source_code
    s3Input:
      Local Path:   /opt/ml/processing/code
      s3DataType:   S3Prefix
      s3InputMode:  File
      s3Uri:        s3://<s3bucket>/mnist_kmeans_example/processing_code/kmeans_preprocessing.py
  Processing Output Config:
    Outputs:
      Output Name:  train_data
      s3Output:
        Local Path:    /opt/ml/processing/output_train/
        s3UploadMode:  EndOfJob
        s3Uri:         s3://<s3bucket>/mnist_kmeans_example/output/
      Output Name:     test_data
      s3Output:
        Local Path:    /opt/ml/processing/output_test/
        s3UploadMode:  EndOfJob
        s3Uri:         s3://<s3bucket>/mnist_kmeans_example/output/
      Output Name:     valid_data
      s3Output:
        Local Path:    /opt/ml/processing/output_valid/
        s3UploadMode:  EndOfJob
        s3Uri:         s3://<s3bucket>/mnist_kmeans_example/output/
  Processing Resources:
    Cluster Config:
      Instance Count:     1
      Instance Type:      ml.m5.xlarge
      Volume Size In GB:  20
  Region:                 us-west-2
  Role Arn:               arn:aws:iam::<acct-id>:role/m-sagemaker-role
  Stopping Condition:
    Max Runtime In Seconds:  1800
  Tags:
    Key:    tagKey
    Value:  tagValue
Status:
  Cloud Watch Log URL:             https://us-west-2.console.aws.amazon.com/cloudwatch/home?region=us-west-2#logStream:group=/aws/sagemaker/ProcessingJobs;prefix=kmeans-mnist-processing-7410ed52fd1811eab19a165ae9f9e385;streamFilter=typeLogStreamPrefix
  Last Check Time:                 2020-09-22T21:14:29Z
  Processing Job Status:           InProgress
  Sage Maker Processing Job Name:  kmeans-mnist-processing-7410ed52fd1811eab19a165ae9f9e385
Events:                            <none>
```

## Delete a ProcessingJob<a name="kubernetes-processing-job-delete"></a>

When you delete a processing job, the SageMaker processing job is removed from Kubernetes but the job isn't deleted from SageMaker\. If the job status in SageMaker is `InProgress` the job is stopped\. Processing jobs that are stopped do not incur any charges for SageMaker resources\. Use one of the following commands to delete a processing job\. 

For cluster\-scoped installation:

```
kubectl delete processingjob kmeans-mnist-processing
```

For namespace\-scoped installation:

```
kubectl delete processingjob kmeans-mnist-processing -n <NAMESPACE>
```

The output for your processing job should look similar to the following\.

```
processingjob.sagemaker.aws.amazon.com "kmeans-mnist-processing" deleted
```



**Note**  
SageMaker does not delete the processing job\. Stopped jobs continue to show in the SageMaker console\. The `delete` command takes a few minutes to clean up the resources from SageMaker\.