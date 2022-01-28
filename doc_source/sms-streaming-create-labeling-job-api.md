# Example: Use SageMaker API To Create Streaming Labeling Job<a name="sms-streaming-create-labeling-job-api"></a>

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) that you can use to start a streaming labeling job for a built\-in task type in the US East \(N\. Virginia\) Region\. For more details about each parameter below see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. To learn how you can create a labeling job using this API and associated language specific SDKs, see [Create a Labeling Job \(API\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-create-labeling-job-api.html)\.

In this example, note the following parameters:
+ `SnsDataSource` – This parameter appears in `InputConfig` and `OutputConfig` and is used to identify your input and output Amazon SNS topics respectively\. To create a streaming labeling job, you are required to provide an Amazon SNS input topic\. Optionally, you can also provide an Amazon SNS output topic\.
+ `S3DataSource` – This parameter is optional\. Use this parameter if you want to include an input manifest file of data objects that you want labeled as soon as the labeling job starts\.
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-StoppingConditions](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-StoppingConditions) – This parameter is ignored when you create a streaming labeling job\. To learn more about stopping a streaming labeling job, see [Stop a Streaming Labeling Job](sms-streaming-stop-labeling-job.md)\.
+ Streaming labeling jobs do not support automated data labeling\. Do not include the `LabelingJobAlgorithmsConfig` parameter\.

```
response = client.create_labeling_job(
    LabelingJobName= 'example-labeling-job',
    LabelAttributeName='label',
    InputConfig={
        'DataSource': {
            'S3DataSource': {
                'ManifestS3Uri': 's3://bucket/path/manifest-with-input-data.json'
            },
            'SnsDataSource': {
                'SnsTopicArn': 'arn:aws:sns:us-east-1:123456789012:your-sns-input-topic'
            }
        },
        'DataAttributes': {
            'ContentClassifiers': [
                'FreeOfPersonallyIdentifiableInformation'|'FreeOfAdultContent',
            ]
        }
    },
    OutputConfig={
        'S3OutputPath': 's3://bucket/path/file-to-store-output-data',
        'KmsKeyId': 'string',
        'SnsTopicArn': 'arn:aws:sns:us-east-1:123456789012:your-sns-output-topic'
    },
    RoleArn='arn:aws:iam::*:role/*',
    LabelCategoryConfigS3Uri='s3://bucket/path/label-categories.json',
    HumanTaskConfig={
        'WorkteamArn': 'arn:aws:sagemaker:us-east-1:*:workteam/private-crowd/*',
        'UiConfig': {
            'UiTemplateS3Uri': 's3://bucket/path/custom-worker-task-template.html'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-tasktype,
        'TaskKeywords': [
            'Example key word',
        ],
        'TaskTitle': 'Multi-label image classification task',
        'TaskDescription': 'Select all labels that apply to the images shown',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-tasktype'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```
