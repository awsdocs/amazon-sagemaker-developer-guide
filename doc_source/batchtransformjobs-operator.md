# BatchTransformJob operator<a name="batchtransformjobs-operator"></a>

Batch transform job operators reconcile your specified batch transform job spec to SageMaker by launching it in SageMaker\. You can learn more about SageMaker batch transform job in the SageMaker [CreateTransformJob API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html)\. 

**Topics**
+ [Create a BatchTransformJob Using a YAML File](#create-a-batchtransformjob-using-a-simple-yaml-file)
+ [Create a BatchTransformJob Using a Helm Chart](#create-a-batchtransformjob-using-a-helm-chart)
+ [List BatchTransformJobs](#list-batch-transform-jobs)
+ [Describe a BatchTransformJob](#describe-a-batch-transform-job)
+ [View Logs from BatchTransformJobs](#view-logs-from-batch-transform-jobs)
+ [Delete a BatchTransformJob](#delete-a-batch-transform-job)

## Create a BatchTransformJob Using a YAML File<a name="create-a-batchtransformjob-using-a-simple-yaml-file"></a>

1. Download the sample YAML file for the batch transform job using the following command: 

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/xgboost-mnist-batchtransform.yaml
   ```

1. Edit the file `xgboost-mnist-batchtransform.yaml` to change necessary parameters to replace the `inputdataconfig` with your input data and `s3OutputPath` with your Amazon S3 buckets that the SageMaker execution role has write access to\. 

1. Apply the YAML file using the following command: 

   ```
   kubectl apply -f xgboost-mnist-batchtransform.yaml
   ```

## Create a BatchTransformJob Using a Helm Chart<a name="create-a-batchtransformjob-using-a-helm-chart"></a>

You can use Helm Charts to run batch transform jobs\. 

### Get the Helm installer directory<a name="get-the-helm-installer-directory"></a>

Clone the GitHub repository to get the source using the following command: 

```
git clone https://github.com/aws/amazon-sagemaker-operator-for-k8s.git
```

### Configure the Helm Chart<a name="configure-the-helm-chart"></a>

Navigate to the `amazon-sagemaker-operator-for-k8s/hack/charts/batch-transform-jobs/` folder\. 

Edit the `values.yaml` file to replace the `inputdataconfig` with your input data and outputPath with your S3 buckets to which the SageMaker execution role has write access\. 

### Create a BatchTransformJob<a name="create-a-batch-transform-job"></a>

1. Use the following command to create a batch transform job: 

   ```
   helm install . --generate-name
   ```

   Your output should look like the following: 

   ```
   NAME: chart-1574292948
   LAST DEPLOYED: Wed Nov 20 23:35:49 2019
   NAMESPACE: default
   STATUS: deployed
   REVISION: 1
   TEST SUITE: None
   NOTES:
   Thanks for installing the sagemaker-k8s-batch-transform-job.
   ```

1. To verify that the Helm Chart was created successfully, run the following command: 

   ```
   helm ls
   NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
   chart-1474292948        default         1               2019-11-20 23:35:49.9136092 +0000 UTC   deployed        sagemaker-k8s-batchtransformjob-0.1.0
   chart-1474292948        default         1               2019-11-20 23:35:49.9136092 +0000 UTC   deployed        sagemaker-k8s-hyperparametertuningjob-0.1.0
   chart-1574292948        default         1               2019-11-20 23:35:49.9136092 +0000 UTC   deployed        sagemaker-k8s-trainingjob-0.1.0
   rolebased-1574291698    default         1               2019-11-20 23:14:59.6777082 +0000 UTC   deployed        sagemaker-k8s-operator-0.1.0
   ```

   This command creates a `BatchTransformJob` Kubernetes resource\. The operator launches the actual transform job in SageMaker and updates the `BatchTransformJob` Kubernetes resource to reflect the status of the job in SageMaker\. You incur charges for SageMaker resources used during the duration of your job\. You do not incur any charges once your job completes or stops\. 

**Note**: SageMaker does not allow you to update a running batch transform job\. You cannot edit any parameter and re\-apply the file/config\. You must either change the metadata name or delete the existing job and create a new one\. Similar to existing training job operators like TFJob in Kubeflow, `update` is not supported\. 

## List BatchTransformJobs<a name="list-batch-transform-jobs"></a>

Use the following command to list all jobs created using the Kubernetes operator: 

```
kubectl get batchtransformjob
```

Your output should look like the following: 

```
NAME                                STATUS      CREATION-TIME          SAGEMAKER-JOB-NAME
xgboost-mnist-batch-transform       Completed   2019-11-18T03:44:00Z   xgboost-mnist-a88fb19809b511eaac440aa8axgboost
```

A batch transform job will continue to be listed after the job has completed or failed\. You can remove a `hyperparametertuningjob` from the list by following the Delete a Batch Transform Job steps\. Jobs that have completed or stopped do not incur any charges for SageMaker resources\. 

### Batch Transform Status Values<a name="batch-transform-status-values"></a>

The `STATUS` field can be one of the following values: 
+ `Completed` 
+ `InProgress` 
+ `Failed` 
+ `Stopped` 
+ `Stopping` 

These statuses come directly from the SageMaker official [API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeHyperParameterTuningJob.html#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)\. 

In addition to the official SageMaker status, it is possible for `STATUS` to be `SynchronizingK8sJobWithSageMaker`\. This means that the operator has not yet processed the job\.

## Describe a BatchTransformJob<a name="describe-a-batch-transform-job"></a>

You can obtain debugging details using the `describe` `kubectl` command\.

```
kubectl describe batchtransformjob xgboost-mnist-batch-transform
```

Your output should look like the following: 

```
Name:         xgboost-mnist-batch-transform
Namespace:    default
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"sagemaker.aws.amazon.com/v1","kind":"BatchTransformJob","metadata":{"annotations":{},"name":"xgboost-mnist","namespace"...
API Version:  sagemaker.aws.amazon.com/v1
Kind:         BatchTransformJob
Metadata:
  Creation Timestamp:  2019-11-18T03:44:00Z
  Finalizers:
    sagemaker-operator-finalizer
  Generation:        2
  Resource Version:  21990924
  Self Link:         /apis/sagemaker.aws.amazon.com/v1/namespaces/default/batchtransformjobs/xgboost-mnist
  UID:               a88fb198-09b5-11ea-ac44-0aa8a9UIDNUM
Spec:
  Model Name:  TrainingJob-20190814SMJOb-IKEB
  Region:      us-east-1
  Transform Input:
    Content Type:  text/csv
    Data Source:
      S 3 Data Source:
        S 3 Data Type:  S3Prefix
        S 3 Uri:        s3://my-bucket/mnist_kmeans_example/input
  Transform Job Name:   xgboost-mnist-a88fb19809b511eaac440aa8a9SMJOB
  Transform Output:
    S 3 Output Path:  s3://my-bucket/mnist_kmeans_example/output
  Transform Resources:
    Instance Count:  1
    Instance Type:   ml.m4.xlarge
Status:
  Last Check Time:                2019-11-19T22:50:40Z
  Sage Maker Transform Job Name:  xgboost-mnist-a88fb19809b511eaac440aaSMJOB
  Transform Job Status:           Completed
Events:                           <none>
```

## View Logs from BatchTransformJobs<a name="view-logs-from-batch-transform-jobs"></a>

Use the following command to see the logs from the `xgboost-mnist` batch transform job: 

```
kubectl smlogs batchtransformjob xgboost-mnist-batch-transform
```

## Delete a BatchTransformJob<a name="delete-a-batch-transform-job"></a>

Use the following command to stop a batch transform job in SageMaker\. 

```
kubectl delete batchTransformJob xgboost-mnist-batch-transform
```

Your output should look like the following: 

```
batchtransformjob.sagemaker.aws.amazon.com "xgboost-mnist" deleted
```

This command removes the batch transform job from your Kubernetes cluster, as well as stops them in SageMaker\. Jobs that have stopped or completed do not incur any charges for SageMaker resources\. Delete takes about 2 minutes to clean up the resources from SageMaker\. 

**Note**: SageMaker does not delete batch transform jobs\. Stopped jobs continue to show on the SageMaker console\. 