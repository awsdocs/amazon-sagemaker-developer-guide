# Neo CLI User Guide for Model Compilation<a name="neo-job-compilation-cli"></a>

This section shows how to manage compilation jobs for machine learning models\. You can create, describe, stop, and list compilation jobs\. 

**Create Compilation Job**

As shown in the following JSON file, you specify the data input format, the S3 bucket where you stored your model, the S3 bucket where you want to write the compiled model, and the target hardware:

```
job.json
{
    "CompilationJobName": "job002",
    "RoleArn": "arn:aws:iam::<your-account>:role/service-role/AmazonSageMaker-ExecutionRole-20180829T140091",
    "InputConfig": {
        "S3Uri": "s3://<your-bucket>/sagemaker/DEMO-breast-cancer-prediction/train",
        "DataInputConfig":  "{\"data\": [1,3,1024,1024]}",
        "Framework": "MXNET"
    },
    "OutputConfig": {
        "S3OutputLocation": "s3://<your-bucket>/sagemaker/DEMO-breast-cancer-prediction/compile",
        "TargetDevice": "ml_c5"
    },
    "StoppingCondition": {
        "MaxRuntimeInSeconds": 300
    }
}


aws sagemaker create-compilation-job \
--cli-input-json file://job.json \
--region us-west-2 

# You should get CompilationJobArn
```

**Describe Compilation Job**

```
aws sagemaker describe-compilation-job \
--compilation-job-name $JOB_NM \
--region us-west-2
```

**Stop Compilation Job**

```
aws sagemaker stop-compilation-job \
--compilation-job-name $JOB_NM \
--region us-west-2

# There is no output for compilation-job operation
```

**List Compilation Job**

```
aws sagemaker list-compilation-jobs \
--region us-west-2
```