# Capture data from batch transform job<a name="model-monitor-data-capture-batch"></a>

The steps required to turn on data capture for your batch transform job are similar whether you use the AWS SDK for Python \(Boto\) or the SageMaker Python SDK\. If you use the AWS SDK, define the [DataCaptureConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html) dictionary, along with required fields, within the `CreateTransformJob` method to turn on data capture\. If you use the SageMaker Python SDK, import the `BatchDataCaptureConfig` class and initialize an instance from this class\. Then, pass this object to the `batch_data_capture_config` parameter of your transform job instance\.

To use the proceeding code snippets, replace the *italicized placeholder text* in the example code with your own information\.

## How to enable data capture<a name="data-capture-batch-enable"></a>

Specify a data capture configuration when you launch a transform job\. Whether you use the AWS SDK for Python \(Boto3\) or the SageMaker Python SDK, you must provide the `DestinationS3Uri` argument, which is the directory where you want the transform job to log the captured data\. Optionally, you can also set the following parameters:
+ `KmsKeyId`: The AWS KMS key used to encrypt the captured data\.
+ `GenerateInferenceId`: A Boolean flag that, when capturing the data, indicates if you want the transform job to append the inference ID and time to your output\. This is useful for model quality monitoring, where you need to ingest the Ground Truth data\. The inference ID and time help to match the captured data with your Ground Truth data\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Configure the data you want to capture with the [DataCaptureConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html) dictionary when you create a transform job using the `CreateTransformJob` method\.

```
input_data_s3_uri = "s3://<input_S3_uri>"
output_data_s3_uri = "s3://<output_S3_uri>"
data_capture_destination = "s3://<captured_data_S3_uri>"

model_name = "<model_name>"

sm_client.create_transform_job(
    TransformJobName="<transform_job_name>",
    MaxConcurrentTransforms=2,
    ModelName=model_name,
    TransformInput={
        'DataSource': {
            'S3DataSource': {
                'S3DataType': "S3Prefix",
                'S3Uri': input_data_s3_uri,
            }
        },
        'ContentType': 'text/csv',
        'CompressionType': 'None',
        'SplitType': 'Line',
    },
    TransformOutput={
        'S3OutputPath': output_data_s3_uri,
        'Accept': 'text/csv',
        'AssembleWith': 'Line',
    },
    TransformResources={
        "InstanceType": "ml.m4.xlarge",
        "InstanceCount": 1,
    },
    DataCaptureConfig={
       "DestinationS3Uri": data_capture_destination,
       "KmsKeyId": "<kms_key>",
       "GenerateInferenceId": True,
    }
)
```

------
#### [ SageMaker Python SDK ]

Import the `BatchDataCaptureConfig` class from the [sagemaker\.model\_monitor](https://sagemaker.readthedocs.io/en/stable/api/inference/model_monitor.html)\.

```
from sagemaker.transformer import Transformer
from sagemaker.inputs import BatchDataCaptureConfig

# Optional - The S3 URI of where to store captured data in S3
data_capture_destination = 's3://<bucket_name>'

model_name = "<inference_model_name>"

transformer = Transformer(
    model_name=<model_name>,
    ...
    batch_data_capture_config=BatchDataCaptureConfig(
        destination_s3_uri=data_capture_destination,
        kms_key_id="<kms_key>",
        generate_inference_id=True,
    )),
    ...
)

transformer.transform(...)
```

------

## How to view data captured<a name="data-capture-batch-view"></a>

Once the transform job completes, the captured data gets logged under the `DestinationS3Uri` you provided with the data capture configuration\. There are two subdirectories under `DestinationS3Uri`, `/input` and `/output`\. If `DestinationS3Uri` is `s3://my-data-capture`, then the transform job creates the the following directories:
+ `s3://my-data-capture/input`: The captured input data for the transform job\.
+ `s3://my-data-capture/output`: The captured output data for the transform job\.

To avoid data duplication, the captured data under the preceding two directories are manifests\. Each manifest is a JSONL file that contains the Amazon S3 locations of the source objects\. A manifest file may look like the following example:

```
// under "/input" directory
[
    {"prefix":"s3://<input_S3_uri>/"},
    "dummy_0.csv",
    "dummy_1.csv",
    "dummy_2.csv",
    ...
]

// under "/output" directory
[
    {"prefix":"s3://<output_S3_uri>/"},
    "dummy_0.csv.out",
    "dummy_1.csv.out",
    "dummy_2.csv.out",
    ...
]
```

The transform job organizes and labels these manifests with a *yyyy/mm/dd/hh* S3 prefix to indicate when they were captured\. This helps the model monitor determine the appropriate portion of data to analyze\. For example, if you start your transform job at 2022\-8\-26 13PM UTC, then the captured data is labeled with a `2022/08/26/13/` prefix string\.

## InferenceId Generation<a name="data-capture-batch-inferenceid"></a>

When you configure `DataCaptureConfig` for a transform job, you can turn on the Boolean flag `GenerateInferenceId`\. This is particularly useful when you need to run model quality and model bias monitoring jobs, for which you need user\-ingested Ground Truth data\. Model monitor relies on an inference ID to match the captured data and the Ground Truth data\. For addition details about Ground Truth ingestion, see [Ingest Ground Truth Labels and Merge Them With Predictions](model-monitor-model-quality-merge.md)\. When `GenerateInferenceId` is on, the transform output appends an inference ID \(a random UUID\) as well as the transform job start time in UTC for each record\. You need these two values to run model quality and model bias monitoring\. When you construct the Ground Truth data, you need to provide the same inference ID to match the output data\. Currently, this feature supports transform outputs in CSV, JSON, and JSONL formats\.

If your transform output is in CSV format, the output file looks like the following example:

```
0, 1f1d57b1-2e6f-488c-8c30-db4e6d757861,2022-08-30T00:49:15Z
1, 22445434-0c67-45e9-bb4d-bd1bf26561e6,2022-08-30T00:49:15Z
...
```

The last two columns are inference ID and the transform job start time\. Do not modify them\. The remaining columns are your transform job outputs\.

If your transform output is in JSON or JSONL format, the output file looks like the following example:

```
{"output": 0, "SageMakerInferenceId": "1f1d57b1-2e6f-488c-8c30-db4e6d757861", "SageMakerInferenceTime": "2022-08-30T00:49:15Z"}
{"output": 1, "SageMakerInferenceId": "22445434-0c67-45e9-bb4d-bd1bf26561e6", "SageMakerInferenceTime": "2022-08-30T00:49:15Z"}
...
```

There are two appended fields that are reserved, `SageMakerInferenceId` and `SageMakerInferenceTime`\. Do not modify these fields if you need to run model quality or model bias monitoring — you need them for merge jobs\.