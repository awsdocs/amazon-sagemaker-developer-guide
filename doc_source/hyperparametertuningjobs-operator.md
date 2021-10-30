# HyperParameterTuningJob operator<a name="hyperparametertuningjobs-operator"></a>

Hyperparameter tuning job operators reconcile your specified hyperparameter tuning job spec to SageMaker by launching it in SageMaker\. You can learn more about SageMaker hyperparameter tuning jobs in the SageMaker [CreateHyperParameterTuningJob API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateHyperParameterTuningJob.html)\. 

**Topics**
+ [Create a HyperparameterTuningJob Using a YAML File](#create-a-hyperparametertuningjob-using-a-simple-yaml-file)
+ [Create a HyperparameterTuningJob using a Helm Chart](#create-a-hyperparametertuningjob-using-a-helm-chart)
+ [List HyperparameterTuningJobs](#list-hyperparameter-tuning-jobs)
+ [Describe a HyperparameterTuningJob](#describe-a-hyperparameter-tuning-job)
+ [View Logs from HyperparameterTuningJobs](#view-logs-from-hyperparametertuning-jobs)
+ [Delete a HyperparameterTuningJob](#delete-hyperparametertuning-jobs)

## Create a HyperparameterTuningJob Using a YAML File<a name="create-a-hyperparametertuningjob-using-a-simple-yaml-file"></a>

1. Download the sample YAML file for the hyperparameter tuning job using the following command: 

   ```
   wget https://raw.githubusercontent.com/aws/amazon-sagemaker-operator-for-k8s/master/samples/xgboost-mnist-hpo.yaml
   ```

1. Edit the `xgboost-mnist-hpo.yaml` file to replace the `roleArn` parameter with your `sagemaker-execution-role`\. For the hyperparameter tuning job to succeed, you must also change the `s3InputPath` and `s3OutputPath` to values that correspond to your account\. Apply the updates YAML file using the following command: 

   ```
   kubectl apply -f xgboost-mnist-hpo.yaml
   ```

## Create a HyperparameterTuningJob using a Helm Chart<a name="create-a-hyperparametertuningjob-using-a-helm-chart"></a>

You can use Helm Charts to run hyperparameter tuning jobs\. 

1. Clone the GitHub repository to get the source using the following command: 

   ```
   git clone https://github.com/aws/amazon-sagemaker-operator-for-k8s.git
   ```

1. Navigate to the `amazon-sagemaker-operator-for-k8s/hack/charts/hyperparameter-tuning-jobs/` folder\. 

1. Edit the `values.yaml` file to replace the `roleArn` parameter with your `sagemaker-execution-role`\. For the hyperparameter tuning job to succeed, you must also change the `s3InputPath` and `s3OutputPath` to values that correspond to your account\. 

### Create the HyperparameterTuningJob<a name="create-the-hpo-job"></a>

With the roles and Amazon S3 paths replaced with appropriate values in `values.yaml`, you can create a hyperparameter tuning job using the following command: 

```
helm install . --generate-name
```

Your output should look similar to the following: 

```
NAME: chart-1574292948
LAST DEPLOYED: Wed Nov 20 23:35:49 2019
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thanks for installing the sagemaker-k8s-hyperparametertuningjob.
```

### Verify Chart Installation<a name="verify-chart-installation"></a>

To verify that the Helm Chart was created successfully, run the following command: 

```
helm ls
```

Your output should look like the following: 

```
NAME                    NAMESPACE       REVISION        UPDATED
chart-1474292948        default         1               2019-11-20 23:35:49.9136092 +0000 UTC   deployed        sagemaker-k8s-hyperparametertuningjob-0.1.0                               STATUS          CHART                           APP VERSION
chart-1574292948        default         1               2019-11-20 23:35:49.9136092 +0000 UTC   deployed        sagemaker-k8s-trainingjob-0.1.0
rolebased-1574291698    default         1               2019-11-20 23:14:59.6777082 +0000 UTC   deployed        sagemaker-k8s-operator-0.1.0
```

`helm install` creates a `HyperParameterTuningJob` Kubernetes resource\. The operator launches the actual hyperparameter optimization job in SageMaker and updates the `HyperParameterTuningJob` Kubernetes resource to reflect the status of the job in SageMaker\. You incur charges for SageMaker resources used during the duration of your job\. You do not incur any charges once your job completes or stops\. 

**Note**: SageMaker does not allow you to update a running hyperparameter tuning job\. You cannot edit any parameter and re\-apply the file/config\. You must either change the metadata name or delete the existing job and create a new one\. Similar to existing training job operators like TFJob in Kubeflow, `update` is not supported\. 

## List HyperparameterTuningJobs<a name="list-hyperparameter-tuning-jobs"></a>

Use the following command to list all jobs created using the Kubernetes operator: 

```
kubectl get hyperparametertuningjob
```

Your output should look like the following: 

```
NAME         STATUS      CREATION-TIME          COMPLETED   INPROGRESS   ERRORS   STOPPED   BEST-TRAINING-JOB                               SAGEMAKER-JOB-NAME
xgboost-mnist-hpo   Completed   2019-10-17T01:15:52Z   10          0            0        0         xgboostha92f5e3cf07b11e9bf6c06d6-009-4c7a123   xgboostha92f5e3cf07b11e9bf6c123
```

A hyperparameter tuning job continues to be listed after the job has completed or failed\. You can remove a `hyperparametertuningjob` from the list by following the steps in Delete a Hyperparameter Tuning Job\. Jobs that have completed or stopped do not incur any charges for SageMaker resources\. 

### Hyperparameter Tuning Job Status Values<a name="hyperparameter-tuning-job-status-values"></a>

The `STATUS` field can be one of the following values: 
+ `Completed` 
+ `InProgress` 
+ `Failed` 
+ `Stopped` 
+ `Stopping` 

These statuses come directly from the SageMaker official [API documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeHyperParameterTuningJob.html#SageMaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)\. 

In addition to the official SageMaker status, it is possible for `STATUS` to be `SynchronizingK8sJobWithSageMaker`\. This means that the operator has not yet processed the job\. 

### Status Counters<a name="status-counters"></a>

The output has several counters, like `COMPLETED` and `INPROGRESS`\. These represent how many training jobs have completed and are in progress, respectively\. For more information about how these are determined, see [TrainingJobStatusCounters](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TrainingJobStatusCounters.html) in the SageMaker API documentation\. 

### Best TrainingJob<a name="best-training-job"></a>

This column contains the name of the `TrainingJob` that best optimized the selected metric\. 

To see a summary of the tuned hyperparameters, run: 

```
kubectl describe hyperparametertuningjob xgboost-mnist-hpo
```

To see detailed information about the `TrainingJob`, run: 

```
kubectl describe trainingjobs <job name>
```

### Spawned TrainingJobs<a name="spawned-training-jobs"></a>

You can also track all 10 training jobs in Kubernetes launched by `HyperparameterTuningJob` by running the following command: 

```
kubectl get trainingjobs
```

## Describe a HyperparameterTuningJob<a name="describe-a-hyperparameter-tuning-job"></a>

You can obtain debugging details using the `describe` `kubectl` command\.

```
kubectl describe hyperparametertuningjob xgboost-mnist-hpo
```

In addition to information about the tuning job, the SageMaker Operator for Kubernetes also exposes the [best training job](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-monitor.html#automatic-model-tuning-best-training-job) found by the hyperparameter tuning job in the `describe` output as follows: 

```
Name:         xgboost-mnist-hpo
Namespace:    default
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"sagemaker.aws.amazon.com/v1","kind":"HyperparameterTuningJob","metadata":{"annotations":{},"name":"xgboost-mnist-hpo","namespace":...
API Version:  sagemaker.aws.amazon.com/v1
Kind:         HyperparameterTuningJob
Metadata:
  Creation Timestamp:  2019-10-17T01:15:52Z
  Finalizers:
    sagemaker-operator-finalizer
  Generation:        2
  Resource Version:  8167
  Self Link:         /apis/sagemaker.aws.amazon.com/v1/namespaces/default/hyperparametertuningjobs/xgboost-mnist-hpo
  UID:               a92f5e3c-f07b-11e9-bf6c-06d6f303uidu
Spec:
  Hyper Parameter Tuning Job Config:
    Hyper Parameter Tuning Job Objective:
      Metric Name:  validation:error
      Type:         Minimize
    Parameter Ranges:
      Integer Parameter Ranges:
        Max Value:     20
        Min Value:     10
        Name:          num_round
        Scaling Type:  Linear
    Resource Limits:
      Max Number Of Training Jobs:     10
      Max Parallel Training Jobs:      10
    Strategy:                          Bayesian
    Training Job Early Stopping Type:  Off
  Hyper Parameter Tuning Job Name:     xgboostha92f5e3cf07b11e9bf6c06d6
  Region:                              us-east-2
  Training Job Definition:
    Algorithm Specification:
      Training Image:       12345678910.dkr.ecr.us-east-2.amazonaws.com/xgboost:1
      Training Input Mode:  File
    Input Data Config:
      Channel Name:  train
      Content Type:  text/csv
      Data Source:
        s3DataSource:
          s3DataDistributionType:  FullyReplicated
          s3DataType:              S3Prefix
          s3Uri:                   https://s3-us-east-2.amazonaws.com/my-bucket/sagemaker/xgboost-mnist/train/
      Channel Name:                validation
      Content Type:                text/csv
      Data Source:
        s3DataSource:
          s3DataDistributionType:  FullyReplicated
          s3DataType:              S3Prefix
          s3Uri:                   https://s3-us-east-2.amazonaws.com/my-bucket/sagemaker/xgboost-mnist/validation/
    Output Data Config:
      s3OutputPath:  https://s3-us-east-2.amazonaws.com/my-bucket/sagemaker/xgboost-mnist/xgboost
    Resource Config:
      Instance Count:     1
      Instance Type:      ml.m4.xlarge
      Volume Size In GB:  5
    Role Arn:             arn:aws:iam::123456789012:role/service-role/AmazonSageMaker-ExecutionRole
    Static Hyper Parameters:
      Name:   base_score
      Value:  0.5
      Name:   booster
      Value:  gbtree
      Name:   csv_weights
      Value:  0
      Name:   dsplit
      Value:  row
      Name:   grow_policy
      Value:  depthwise
      Name:   lambda_bias
      Value:  0.0
      Name:   max_bin
      Value:  256
      Name:   max_leaves
      Value:  0
      Name:   normalize_type
      Value:  tree
      Name:   objective
      Value:  reg:linear
      Name:   one_drop
      Value:  0
      Name:   prob_buffer_row
      Value:  1.0
      Name:   process_type
      Value:  default
      Name:   rate_drop
      Value:  0.0
      Name:   refresh_leaf
      Value:  1
      Name:   sample_type
      Value:  uniform
      Name:   scale_pos_weight
      Value:  1.0
      Name:   silent
      Value:  0
      Name:   sketch_eps
      Value:  0.03
      Name:   skip_drop
      Value:  0.0
      Name:   tree_method
      Value:  auto
      Name:   tweedie_variance_power
      Value:  1.5
    Stopping Condition:
      Max Runtime In Seconds:  86400
Status:
  Best Training Job:
    Creation Time:  2019-10-17T01:16:14Z
    Final Hyper Parameter Tuning Job Objective Metric:
      Metric Name:        validation:error
      Value:
    Objective Status:     Succeeded
    Training End Time:    2019-10-17T01:20:24Z
    Training Job Arn:     arn:aws:sagemaker:us-east-2:123456789012:training-job/xgboostha92f5e3cf07b11e9bf6c06d6-009-4sample
    Training Job Name:    xgboostha92f5e3cf07b11e9bf6c06d6-009-4c7a3059
    Training Job Status:  Completed
    Training Start Time:  2019-10-17T01:18:35Z
    Tuned Hyper Parameters:
      Name:                                    num_round
      Value:                                   18
  Hyper Parameter Tuning Job Status:           Completed
  Last Check Time:                             2019-10-17T01:21:01Z
  Sage Maker Hyper Parameter Tuning Job Name:  xgboostha92f5e3cf07b11e9bf6c06d6
  Training Job Status Counters:
    Completed:            10
    In Progress:          0
    Non Retryable Error:  0
    Retryable Error:      0
    Stopped:              0
    Total Error:          0
Events:                   <none>
```

## View Logs from HyperparameterTuningJobs<a name="view-logs-from-hyperparametertuning-jobs"></a>

Hyperparameter tuning jobs do not have logs, but all training jobs launched by them do have logs\. These logs can be accessed as if they were a normal training job\. For more information, see View Logs from Training Jobs\. 

## Delete a HyperparameterTuningJob<a name="delete-hyperparametertuning-jobs"></a>

Use the following command to stop a hyperparameter job in SageMaker\. 

```
kubectl delete hyperparametertuningjob xgboost-mnist-hpo
```

This command removes the hyperparameter tuning job and associated training jobs from your Kubernetes cluster and stops them in SageMaker\. Jobs that have stopped or completed do not incur any charges for SageMaker resources\. SageMaker does not delete hyperparameter tuning jobs\. Stopped jobs continue to show on the SageMaker Console\. 

Your output should look like the following: 

```
hyperparametertuningjob.sagemaker.aws.amazon.com "xgboost-mnist-hpo" deleted
```

**Note**: The delete command takes about 2 minutes to clean up the resources from SageMaker\. 