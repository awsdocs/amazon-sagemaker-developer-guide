# create\_tuning\_job<a name="API_create-tuning-job"></a>

Starts a hyperparameter tuning job\.

## Request Syntax<a name="API_create-tuning-job-syntax"></a>

```
request = client.create_tuning_job(
     TuningJobName = 'string',
     
     AwsAccountId = 'string,
     
     TuningJobConfig = {
        "Strategy": "Bayesian",
        "TuningJobObjective": {
           "Type": "Maximize"|"Minimize",
           "MetricName": "string"
        },
        "ResourceLimits": {
        "MaxNumberOfTrainingJobs": integer,
        "MaxParallelTrainingJobs": integer,
        "MaxClockTimeInHours": integer
        },
        "ParameterRanges": {
           "IntegerParameterRanges": [
              {
                 "Name": "string",
                 "Type": "Integer"|"Continuous"|"Categorical",
                 "MinValue": "string",
                 "MaxValue": "string"
              }
            ...
            ],
            "ContinuousParameterRanges": [
                {
                   "Name": "string",
                   "Type": "Integer"|"Continuous"|"Categorical",
                   "MinValue": "string",
                   "MaxValue": "string"
                }
             ...
             ],
            "CategoricalParameterRanges": [
               {
                  "Name": "string",
                  "Type": "Integer"|"Continuous"|"Categorical",
                  "Values": ["string", ...]
               }
            ...
            ]
         }
      }
      
      TrainingJobDefinition = {
    "StaticHyperParameters": {"string": "string"...},
    "AlgorithmSpecification": {
        "TrainingImage": "string",
        "TrainingInputMode": "Pipe"|"File",
        "MetricDefinitions": [
            {
                "Name": "string",
                "Regex": "string"
            }
            ...
        ]
    },
    "RoleArn": "string",
    "InputDataConfig": [
        {
            "ChannelName": "string",
            "DataSource": 
            {
                "S3DataSource": 
                {
                    "S3DataType": "ManifestFile"|"S3Prefix",
                    "S3Uri": "string",
                    "S3DataDistributionType": "FullyReplicated"|"ShardedByS3Key"
                }
            },
            "ContentType": "string",
            "CompressionType": "None"|"Gzip",
            "RecordWrapperType": "None"|"RecordIO"
        }
        ...
    ],
    "OutputDataConfig": {
        "KmsKeyId": "string",
        "S3OutputPath": "string"
    },
    "ResourceConfig": {
        "InstanceType": "string",
        "InstanceCount": integer,
        "VolumeSizeInGB": integer
    },
    "StoppingCondition": {
        "MaxRuntimeInSeconds": integer
    }
}

Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

## Parameters<a name="API_create-tuning-job-parameters"></a>

**TuningJobName**  
Name for the tuning job\. This name is the prefix for the names of all training jobs that this tuning job launches\. The name must be unique within the same account and region\. Names are case insensitive, and must be between 1\-32 characters in length\.

**AwsAccountId**  
The AWS account ID of the caller\.

**TuningJobConfig**  
A JSON object that descrubes the tuning job, including the search strategy, the metric used to evaluate training jobs, ranges of parameters to search, and resource limits for the tuning job\.

**TrainingJobDefinition**  
A JSON object that describes the training jobs that this tuning job launches, including static hyperparameters, input data configuration, output data configuration, resource configuration, and stopping condition\.

**Tags**  
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com//awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.

## Response Syntax<a name="API_create-tuning-job-response"></a>

```
{
       'TuningJobArn': 'string'
    }
```

## Response Parameters<a name="API_create-tuning-job-response-params"></a>

**TuningJobArn**  
The Amazon Resource Name \(ARN\) of the tuning job\.