# list\_training\_jobs\_for\_tuning\_job<a name="API_list-training-jobs-for-tuning-job"></a>

Gets a list of the training jobs that this tuning job launched\.

## Request Syntax<a name="API_list-training-jobs-for-tuning-jobb-syntax"></a>

```
request = client.list_training_jobs_for_tuning_job(
     TuningJobName = 'string',
     
     MaxResults = integer,
     
     AwsAccountId = 'string'
     
     )
```

## Parameters<a name="API_list-training-jobs-for-tuning-job-parameters"></a>

**TuningJobName**  
Name of the tuning job\.

**MaxResults**  
The maximum number of training jobs to list\.

**AwsAccountId**  
The AWS account ID of the caller\.

## Response Syntax<a name="API_list-training-jobs-for-tuning-job-response"></a>

```
'TrainingJobSummaries': [
    {
        'FinalTuningJobObjectiveMetric': 
        {
            'MetricName': 'string',
            'Value': float
        },
        'HyperParameters': 
        {
            'string': 'string',
            ...
        },
        'RetriedAttempt': integer,
        'TrainingJobName': 'string',
        'TrainingJobStatus': 'InProgress'|'Completed'|'Failed'|'Stopping'|'Stopped'
    },
    ...
]
```

## Response Parameters<a name="API_list-training-jobs-for-tuning-job-response-params"></a>

**TrainingJobSummaries**  
A list of summaries of training jobs launched by this tuning job\.    
**FinalTuningJobObjectiveMetric**  
The objective metric of the best training job for this tuning job\.    
MetricName  
The name of the objective metric\. Must be between 1\-255 characters\.  
Value  
The value of the objective metric\.  
**HyperParameters**  
An array of the hyperparameters that this tuning job tuned\.    
Name  
The name of the hyperparameter\.  
Value  
The value of the hyperparameter\.  
**RetriedAttempt**  
The number of attempts to retry the training job\.  
**TrainingJobName**  
The name of the training job\.  
**TrainingJobStatus**  
The status of the training job\. One of *InProgress*, *Completed*, *Failed*, *Stopping*, or *Stopped*