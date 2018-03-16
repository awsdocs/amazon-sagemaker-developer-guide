# describe\_tuning\_job<a name="API_describe-tuning-job"></a>

Describes a hyperparameter tuning job\.

## Request Syntax<a name="API_describe-tuning-job-syntax"></a>

```
request = client.describe_tuning_job(
     TuningJobName = 'string',
     
     AwsAccountId = 'string'
     
     )
```

## Parameters<a name="API_describe-tuning-job-parameters"></a>

**TuningJobName**  
Name of the tuning job to describe\.

**AwsAccountId**  
The AWS account ID of the caller\.

## Response Syntax<a name="API_describe-tuning-job-response"></a>

```
{
    'BestTrainingJob': 
    {
        'FinalTuningJobObjectiveMetric': {
            'MetricName': 'string',
            'Value': 'float'
        },
        'HyperParameters': 
        {
            'string': 'string',
            ...
        },
        'RetriedAttempt': 'integer',
        'TrainingJobName': 'string',
        'TrainingJobStatus': 'InProgress'|'Completed'|'Failed'|'Stopping'|'Stopped',
    },
    
    'MetricDefinitions': [
        {
            'Name': 'string',
            'Regex': 'string'
        },
        ...
    ],
    'ParameterRanges': 
    {
        'CategoricalParameterRanges': [
            {
                'Name': 'string',
                'Type': 'Integer'|'Continuous'|'Categorical',
                'Values': ['string', ...]
            },
            ...
        ],
        'ContinuousParameterRanges': [
            {
                'Name': 'string',
                'Type': 'Integer'|'Continuous'|'Categorical',
                'MinValue': 'string',
                'MaxValue': 'string'
            },
            ...
        ],
        'IntegerParameterRanges': [
            {
                'Name': 'string',
                'Type': 'Integer'|'Continuous'|'Categorical',
                'MinValue': 'string',
                'MaxValue': 'string'
            },
            ...
        ]
    },
    'ResourcesStats': 
    {
        'ConsumedResources': 
        {
            'ComputeHours': 
            {
                'Finished': integer,
                'InFlight': integer
            },
            'Jobs': 
            {
                'Completed': integer, 
                'Failed': integer, 
                'Pending': integer, 
                'Running': integer
            },
            'ResourceLimits': 
            {
                'MaxClockTimeInHours': integer,
                'MaxNumberOfTrainingJobs': integer,
                'MaxParallelTrainingJobs': integer
            }
            
        }
    },
    'Strategy': 'Bayesian',
    'TuningJobName': 'mytuningjobtestbig',
    'TuningJobStartTime': datetime,
    'TuningJobStatus': 'InProgress'|'Completed'|'Failed'|'Stopping'|'Stopped'
    'FailureReason': 'string'
}
```

## Response Parameters<a name="API_describe-tuning-job-response-params"></a>

**BestTrainingJob**  
Description of the trainging job that resulted in the best objective measure for this tuning job\.    
**FinalTuningJobObjectiveMetric**  
The objective metric of the best traingin job for this tuning job\.    
MetricName  
The name of the objective metric\. Must be between 1\-255 characters\.  
Value  
The value of the objective metric\.  
**HyperParameters**  
An Array of key\-value pairs that represent the hyperparameters of the besttraining job for this tuning job\.  
**RetriedAttempt**  
The number of retry attempts made for the training job\.  
**TrainingJobName**  
The name of the training job\.  
**TrainingJobStatus**  
The status of the training job\.

**MetricDefinitions**  
List of metric definitions\. Each item in this list describes a metric that the training job outputs to `stdout` or `stderr`\. These metrics are caputred in CloudWatch logs\. For more information, see [Logging Amazon SageMaker with Amazon CloudWatch](logging-cloudwatch.md)"     
Name  
The name of the metric\.  
Regex  
A regular expression used to capture the metric from training job output\.

**ParameterRanges**  
Defines the ranges of hyperparameters to be tuned\. These include `Categorical`, `Integer`, and `Continuous` hyperparameters\.    
Categorical  
A list of categorical hyperparameters to tune\.    
Name  
The name of the categorical hyperparameter to tune\.  
Type  
The type of the hyperparameter\. Set this to *Categorical*\.  
Values  
A list of the categories for the hyperparameter\.  
Continuous  
A list of continuous hyperparameters to tune\.    
MaxValue  
The maximum value for the hyperparameter\. The tuning job uses float values between `MinValue` value and this value for tuning\.  
MinValue  
The minimum value for the hyperparameter\. The tuning job uses float values between this value and `MaxValue`for tuning\.  
Name  
The name of the hyperparameter to tune\.  
Type  
The type of the hyperparameter\. Set this to *Continuous*\.  
Intger  
A list of integer hyperparameters to tune\.    
MaxValue  
The maximum value for the hyperparameter\. The tuning job uses integer values between `MinValue` value and this value for tuning\.  
MinValue  
The minimum value for the hyperparameter\. The tuning job uses integer values between this value and `MaxValue`for tuning\.  
Name  
The name of the hyperparameter to tune\.  
Type  
The type of the hyperparameter\. Set this to *Intger*\.

**ResourceStats**  
Details of resources used for the tuning job\.    
ConsumedResources  
Compute hours consumed by the tuning job\.    
ComputeHours  
Compute hours consumed by the tunig job\.    
Finished  
Completd compute hours consumed by the tuning job\.  
InFlight  
In flight compute hours consumed by the tuning job\.  
Jobs  
The number of training jobs launched by this tuning job\.    
Completed  
The number of successfully completed training jobs launched by this tuning job\.  
Failed  
The number of failed training jobs launched by this tuning job\.  
Pending  
The number of pending training jobs launched by this tuning job\.  
Running  
The number of currently\-running training jobs launched by this tuning job\.  
ResourceLimits  
The resource limits defined for this tuning job\.    
MaxClockTimeInHours  
The maximum number of hours of clock time allowed for this tuning job\.  
MaxNumberOfTrainingJobs  
The maximum number of training jobs this tuning job is allowed to launch\.  
MaxParallelTrainingJobs  
The maximum number of parallel training jobs this tuning job is allowed to launch\.

**Strategy**  
The hyperparameter search strategy to use for the tuning job\. Set this to *Bayesian*

**TuningJobEndTime**  
The date and time that the tuning job ended\.

**TuningJobName**  
The name of the tuning job\.

**TuningJobStartTime**  
The date and time that the tuning job started\.

**TuningJobStatus**  
The status of the tuning job\. One of *InProgress*, *Completed*, *Failed*, *Stopping*, or *Stopped*