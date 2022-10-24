# Batch inferencing<a name="autopilot-deploy-models-batch"></a>

Batch inferencing, also known as offline inferencing, generates model predictions on a batch of observations\. Batch inference is a good option if you don't need an immediate response to a model prediction request\. 

Offline predictions are suitable for large datasets or when you need to wait a long time for a response\. By contrast, online inference, or [real\-time inferencing](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-deploy-models.html#autopilot-deploy-models-realtime), generates predictions in real time\. 

You can make batch inferences from an Autopilot model using the [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/), the [AWS SDK for Python \(Boto3\)](http://aws.amazon.com/sdk-for-python/) or [AWS CLI](https://docs.aws.amazon.com/cli/)\.

## Batch inferencing steps<a name="autopilot-deploy-models-batch-steps"></a>

To use the SageMaker APIs for batch inferencing, there are three general steps:

1. **Obtain candidate definitions** 

   Candidate definitions from [InferenceContainers](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLCandidate.html#sagemaker-Type-AutoMLCandidate-InferenceContainers) are used to create a SageMaker model\. 

   The following example shows how to use the [DescribeAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html) API to obtain candidate definitions for the best model candidate\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker describe-auto-ml-job --auto-ml-job-name <job-name> --region <region>
   ```

   Use the [ListCandidatesForAutoMLJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidatesForAutoMLJob.html) API to list all candidates\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker list-candidates-for-auto-ml-job --auto-ml-job-name <job-name> --region <region>
   ```

1. **Create a SageMaker model**

   Use the container definitions from the previous steps to create a SageMaker model using the [CreateModel](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker create-model --model-name '<your-custom-model-name>' \
                       --containers ['<container-definition1>, <container-definition2>, <container-definition3>]' \
                       --execution-role-arn '<execution-role-arn>' --region '<region>
   ```

1. **Create a SageMaker transform job** 

   The following example creates a SageMaker transform job with the [CreateTransformJob](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-transform-job.html) API\. See the following AWS CLI command as an example\.

   ```
   aws sagemaker create-transform-job --transform-job-name '<your-custom-transform-job-name>' --model-name '<your-custom-model-name-from-last-step>'\
   --transform-input '{
           "DataSource": {
               "S3DataSource": {
                   "S3DataType": "S3Prefix", 
                   "S3Uri": "<your-input-data>" 
               }
           },
           "ContentType": "text/csv",
           "SplitType": "Line"
       }'\
   --transform-output '{
           "S3OutputPath": "<your-output-path>",
           "AssembleWith": "Line" 
       }'\
   --transform-resources '{
           "InstanceType": "<instance-type>", 
           "InstanceCount": 1
       }' --region '<region>'
   ```

Check the progress of your transform job using the [DescribeTransformJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html) API\. See the following AWS CLI command as an example\.

```
aws sagemaker describe-transform-job --transform-job-name '<your-custom-transform-job-name>' --region <region>
```

After the job is finished, the predicted result will be available in `<your-output-path>`\. 

The output file name has the following format: `<input_data_file_name>.out`\. As an example, if your input file is `text_x.csv`, the output name will be `text_x.csv.out`\.

The following tabs show code examples for SageMaker Python SDK, AWS SDK for Python \(Boto3\), and AWS CLI\.

------
#### [ SageMaker Python SDK ]

The following example uses the **[SageMaker Python SDK ](https://sagemaker.readthedocs.io/en/stable/overview.html)** to make predictions in batches\.

```
from sagemaker import AutoML

sagemaker_session= sagemaker.session.Session()

job_name = 'test-auto-ml-job' # your autopilot job name
automl = AutoML.attach(auto_ml_job_name=job_name)
output_path = 's3://test-auto-ml-job/output'
input_data = 's3://test-auto-ml-job/test_X.csv'

# call DescribeAutoMLJob API to get the best candidate definition
best_candidate = automl.describe_auto_ml_job()['BestCandidate']
best_candidate_name = best_candidate['CandidateName']

# create model
model = automl.create_model(name=best_candidate_name, 
               candidate=best_candidate)

# create transformer
transformer = model.transformer(instance_count=1, 
    instance_type='ml.m5.2xlarge',
    assemble_with='Line',
    output_path=output_path)

# do batch transform
transformer.transform(data=input_data,
                      split_type='Line',
                       content_type='text/csv',
                       wait=True)
```

------
#### [ AWS SDK for Python \(Boto3\) ]

 The following example uses **AWS SDK for Python \(Boto3\)** to make predictions in batches\.

```
import sagemaker 
import boto3

session = sagemaker.session.Session()

sm_client = boto3.client('sagemaker', region_name='us-west-2')
role = 'arn:aws:iam::1234567890:role/sagemaker-execution-role'
output_path = 's3://test-auto-ml-job/output'
input_data = 's3://test-auto-ml-job/test_X.csv'

best_candidate = sm_client.describe_auto_ml_job(AutoMLJobName=job_name)['BestCandidate']
best_candidate_containers = best_candidate['InferenceContainers']
best_candidate_name = best_candidate['CandidateName']

# create model
reponse = sm_client.create_model(
    ModelName = best_candidate_name,
    ExecutionRoleArn = role,
    Containers = best_candidate_containers 
)

# Lauch Transform Job
response = sm_client.create_transform_job(
    TransformJobName=f'{best_candidate_name}-transform-job',
    ModelName=model_name,
    TransformInput={
        'DataSource': {
            'S3DataSource': {
                'S3DataType': 'S3Prefix',
                'S3Uri': input_data
            }
        },
        'ContentType': "text/csv",
        'SplitType': 'Line'
    },
    TransformOutput={
        'S3OutputPath': output_path,
        'AssembleWith': 'Line',
    },
    TransformResources={
        'InstanceType': 'ml.m5.2xlarge',
        'InstanceCount': 1,
    },
)
```

The batch inference job returns a response in the following format\.

```
{'TransformJobArn': 'arn:aws:sagemaker:us-west-2:1234567890:transform-job/test-transform-job',
 'ResponseMetadata': {'RequestId': '659f97fc-28c4-440b-b957-a49733f7c2f2',
  'HTTPStatusCode': 200,
  'HTTPHeaders': {'x-amzn-requestid': '659f97fc-28c4-440b-b957-a49733f7c2f2',
   'content-type': 'application/x-amz-json-1.1',
   'content-length': '96',
   'date': 'Thu, 11 Aug 2022 22:23:49 GMT'},
  'RetryAttempts': 0}}
```

------
#### [ AWS Command Line Interface \(AWS CLI\) ]

1. **Obtain the candidate definitions** by using the following the code example\.

   ```
   aws sagemaker describe-auto-ml-job --auto-ml-job-name 'test-automl-job' --region us-west-2
   ```

1. **Create the model** by using the following the code example\.

   ```
   aws sagemaker create-model --model-name 'test-sagemaker-model'
   --containers '[{
       "Image": "348316444620.dkr.ecr.us-west-2.amazonaws.com/sagemaker-sklearn-automl:2.5-1-cpu-py3",
       "ModelDataUrl": "s3://test-bucket/out/test-job1/data-processor-models/test-job1-dpp0-1-e569ff7ad77f4e55a7e549a/output/model.tar.gz",
       "Environment": {
           "AUTOML_SPARSE_ENCODE_RECORDIO_PROTOBUF": "1",
           "AUTOML_TRANSFORM_MODE": "feature-transform",
           "SAGEMAKER_DEFAULT_INVOCATIONS_ACCEPT": "application/x-recordio-protobuf",
           "SAGEMAKER_PROGRAM": "sagemaker_serve",
           "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code"
       }
   }, {
       "Image": "348316444620.dkr.ecr.us-west-2.amazonaws.com/sagemaker-xgboost:1.3-1-cpu-py3",
       "ModelDataUrl": "s3://test-bucket/out/test-job1/tuning/flicdf10v2-dpp0-xgb/test-job1E9-244-7490a1c0/output/model.tar.gz",
       "Environment": {
           "MAX_CONTENT_LENGTH": "20971520",
           "SAGEMAKER_DEFAULT_INVOCATIONS_ACCEPT": "text/csv",
           "SAGEMAKER_INFERENCE_OUTPUT": "predicted_label", 
           "SAGEMAKER_INFERENCE_SUPPORTED": "predicted_label,probability,probabilities" 
       }
   }, {
       "Image": "348316444620.dkr.ecr.us-west-2.amazonaws.com/sagemaker-sklearn-automl:2.5-1-cpu-py3", 
       "ModelDataUrl": "s3://test-bucket/out/test-job1/data-processor-models/test-job1-dpp0-1-e569ff7ad77f4e55a7e549a/output/model.tar.gz", 
       "Environment": { 
           "AUTOML_TRANSFORM_MODE": "inverse-label-transform", 
           "SAGEMAKER_DEFAULT_INVOCATIONS_ACCEPT": "text/csv", 
           "SAGEMAKER_INFERENCE_INPUT": "predicted_label", 
           "SAGEMAKER_INFERENCE_OUTPUT": "predicted_label", 
           "SAGEMAKER_INFERENCE_SUPPORTED": "predicted_label,probability,labels,probabilities", 
           "SAGEMAKER_PROGRAM": "sagemaker_serve", 
           "SAGEMAKER_SUBMIT_DIRECTORY": "/opt/ml/model/code" 
       } 
   }]' \
   --execution-role-arn 'arn:aws:iam::1234567890:role/sagemaker-execution-role' \
   --region 'us-west-2'
   ```

1. **Create the transform job** by using the following the code example\.

   ```
   aws sagemaker create-transform-job --transform-job-name 'test-tranform-job'\
    --model-name 'test-sagemaker-model'\
   --transform-input '{
           "DataSource": {
               "S3DataSource": {
                   "S3DataType": "S3Prefix",
                   "S3Uri": "s3://test-bucket/data.csv"
               }
           },
           "ContentType": "text/csv",
           "SplitType": "Line"
       }'\
   --transform-output '{
           "S3OutputPath": "s3://test-bucket/output/",
           "AssembleWith": "Line"
       }'\
   --transform-resources '{
           "InstanceType": "ml.m5.2xlarge",
           "InstanceCount": 1
       }'\
   --region 'us-west-2'
   ```

1. **Check the progress of the transform job** by using the following the code example\. 

   ```
   aws sagemaker describe-transform-job --transform-job-name  'test-tranform-job' --region us-west-2
   ```

   The following is the response from the transform job\.

   ```
   {
       "TransformJobName": "test-tranform-job",
       "TransformJobArn": "arn:aws:sagemaker:us-west-2:1234567890:transform-job/test-tranform-job",
       "TransformJobStatus": "InProgress",
       "ModelName": "test-model",
       "TransformInput": {
           "DataSource": {
               "S3DataSource": {
                   "S3DataType": "S3Prefix",
                   "S3Uri": "s3://test-bucket/data.csv"
               }
           },
           "ContentType": "text/csv",
           "CompressionType": "None",
           "SplitType": "Line"
       },
       "TransformOutput": {
           "S3OutputPath": "s3://test-bucket/output/",
           "AssembleWith": "Line",
           "KmsKeyId": ""
       },
       "TransformResources": {
           "InstanceType": "ml.m5.2xlarge",
           "InstanceCount": 1
       },
       "CreationTime": 1662495635.679,
       "TransformStartTime": 1662495847.496,
       "DataProcessing": {
           "InputFilter": "$",
           "OutputFilter": "$",
           "JoinSource": "None"
       }
   }
   ```

   After the `TransformJobStatus` changes to `Completed`, you can check the inference result in the `S3OutputPath`\.

------

## Batch inferencing across accounts<a name="autopilot-deploy-models-batch-across-accounts"></a>

To create a batch inferencing job in a different account than the one that the model was generated in, follow the instructions in [Deploy models from different accounts](autopilot-deploy-models-realtime.md#autopilot-deploy-models-realtime-across-accounts)\. Then you can create models and transform jobs by following the [Batch inferencing steps](#autopilot-deploy-models-batch-steps)\.