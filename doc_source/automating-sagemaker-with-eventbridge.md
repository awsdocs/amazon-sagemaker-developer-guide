# Automating Amazon SageMaker with Amazon EventBridge<a name="automating-sagemaker-with-eventbridge"></a>

Amazon EventBridge monitors status change events in Amazon SageMaker\. EventBridge enables you to automate SageMaker and respond automatically to events such as a training job status change or endpoint status change\. Events from SageMaker are delivered to EventBridge in near real time\. You can write simple rules to indicate which events are of interest to you, and what automated actions to take when an event matches a rule\. For an example of how to create a rule, see [Schedule a Pipeline with Amazon EventBridge](pipeline-eventbridge.md#pipeline-eventbridge-schedule)\.

Some examples of the actions that can be automatically triggered include the following:
+ Invoking an AWS Lambda function
+ Invoking Amazon EC2 Run Command
+ Relaying the event to Amazon Kinesis Data Streams
+ Activating an AWS Step Functions state machine
+ Notifying an Amazon SNS topic or an AWS SMS queue

**Topics**
+ [Training job state change](#eventbridge-training)
+ [Hyperparameter tuning job state change](#eventbridge-hpo)
+ [Transform job state change](#eventbridge-transform)
+ [Endpoint state change](#eventbridge-endpoint)
+ [Feature group state change](#eventbridge-feature-group)
+ [Model package state change](#eventbridge-model-package)
+ [Pipeline execution state change](#eventbridge-pipeline)
+ [Pipeline step state change](#eventbridge-pipeline-step)
+ [SageMaker image state change](#eventbridge-image-state)
+ [SageMaker image version state change](#eventbridge-image-version-state)
+ [Endpoint deployment state change](#eventbridge-deployment-state)
+ [Model card state change](#eventbridge-model-card-state)

## Training job state change<a name="eventbridge-training"></a>

Indicates a change in the status of a SageMaker training job\.

If the value of `TrainingJobStatus` is `Failed`, the event contains the `FailureReason` field, which provides a description of why the training job failed\.

```
{
    "version": "0",
    "id": "844e2571-85d4-695f-b930-0153b71dcb42",
    "detail-type": "SageMaker Training Job State Change",
    "source": "aws.sagemaker",
    "account": "123456789012",
    "time": "2018-10-06T12:26:13Z",
    "region": "us-east-1",
    "resources": [
        "arn:aws:sagemaker:us-east-1:123456789012:training-job/kmeans-1"
    ],
    "detail": {
        "TrainingJobName": "89c96cc8-dded-4739-afcc-6f1dc936701d",
        "TrainingJobArn": "arn:aws:sagemaker:us-east-1:123456789012:training-job/kmeans-1",
        "TrainingJobStatus": "Completed",
        "SecondaryStatus": "Completed",
        "HyperParameters": {
            "Hyper": "Parameters"
        },
        "AlgorithmSpecification": {
            "TrainingImage": "TrainingImage",
            "TrainingInputMode": "TrainingInputMode"
        },
        "RoleArn": "arn:aws:iam::123456789012:role/SMRole",
        "InputDataConfig": [
            {
                "ChannelName": "Train",
                "DataSource": {
                    "S3DataSource": {
                        "S3DataType": "S3DataType",
                        "S3Uri": "S3Uri",
                        "S3DataDistributionType": "S3DataDistributionType"
                    }
                },
                "ContentType": "ContentType",
                "CompressionType": "CompressionType",
                "RecordWrapperType": "RecordWrapperType"
            }
        ],
        "OutputDataConfig": {
            "KmsKeyId": "KmsKeyId",
            "S3OutputPath": "S3OutputPath"
        },
        "ResourceConfig": {
            "InstanceType": "InstanceType",
            "InstanceCount": 3,
            "VolumeSizeInGB": 20,
            "VolumeKmsKeyId": "VolumeKmsKeyId"
        },
        "VpcConfig": {

        },
        "StoppingCondition": {
            "MaxRuntimeInSeconds": 60
        },
        "CreationTime": "1583831889050",
        "TrainingStartTime": "1583831889050",
        "TrainingEndTime": "1583831889050",
        "LastModifiedTime": "1583831889050",
        "SecondaryStatusTransitions": [

        ],
        "Tags": {

        }
    }
}
```

## Hyperparameter tuning job state change<a name="eventbridge-hpo"></a>

Indicates a change in the status of a SageMaker hyperparameter tuning job\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "SageMaker HyperParameter Tuning Job State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:sagemaker:us-east-1:123456789012:tuningJob/x"
  ],
  "detail": {
    "HyperParameterTuningJobName": "016bffd3-6d71-4d3a-9710-0a332b2759fc",
    "HyperParameterTuningJobArn": "arn:aws:sagemaker:us-east-1:123456789012:tuningJob/x",
    "TrainingJobDefinition": {
      "StaticHyperParameters": {},
      "AlgorithmSpecification": {
        "TrainingImage": "trainingImageName",
        "TrainingInputMode": "inputModeFile",
        "MetricDefinitions": [
          {
            "Name": "metricName",
            "Regex": "regex"
          }
        ]
      },
      "RoleArn": "roleArn",
      "InputDataConfig": [
        {
          "ChannelName": "channelName",
          "DataSource": {
            "S3DataSource": {
              "S3DataType": "s3DataType",
              "S3Uri": "s3Uri",
              "S3DataDistributionType": "s3DistributionType"
            }
          },
          "ContentType": "contentType",
          "CompressionType": "gz",
          "RecordWrapperType": "RecordWrapper"
        }
      ],
      "VpcConfig": {
        "SecurityGroupIds": [
          "securityGroupIds"
        ],
        "Subnets": [
          "subnets"
        ]
      },
      "OutputDataConfig": {
        "KmsKeyId": "kmsKeyId",
        "S3OutputPath": "s3OutputPath"
      },
      "ResourceConfig": {
        "InstanceType": "instanceType",
        "InstanceCount": 10,
        "VolumeSizeInGB": 500,
        "VolumeKmsKeyId": "volumeKeyId"
      },
      "StoppingCondition": {
        "MaxRuntimeInSeconds": 3600
      }
    },
    "HyperParameterTuningJobStatus": "status",
    "CreationTime": "1583831889050",
    "LastModifiedTime": "1583831889050",
    "TrainingJobStatusCounters": {
      "Completed": 1,
      "InProgress": 0,
      "RetryableError": 0,
      "NonRetryableError": 0,
      "Stopped": 0
    },
    "ObjectiveStatusCounters": {
      "Succeeded": 1,
      "Pending": 0,
      "Failed": 0
    },
    "Tags": {}
  }
}
```

## Transform job state change<a name="eventbridge-transform"></a>

Indicates a change in the status of a SageMaker batch transform job\.

If the value of `TransformJobStatus` is `Failed`, the event contains the `FailureReason` field, which provides a description of why the training job failed\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "SageMaker Transform Job State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2018-10-06T12:26:13Z",
  "region": "us-east-1",
  "resources": ["arn:aws:sagemaker:us-east-1:123456789012:transform-job/myjob"],
  "detail": {
    "TransformJobName": "4b52bd8f-e034-4345-818d-884bdd7c9724",
    "TransformJobArn": "arn:aws:sagemaker:us-east-1:123456789012:transform-job/myjob",
    "TransformJobStatus": "another status... GO",
    "FailureReason": "failed why 1",
    "ModelName": "i am a beautiful model",
    "MaxConcurrentTransforms": 5,
    "MaxPayloadInMB": 10,
    "BatchStrategy": "Strategizing...",
    "Environment": {
      "environment1": "environment2"
    },
    "TransformInput": {
      "DataSource": {
        "S3DataSource": {
          "S3DataType": "s3DataType",
          "S3Uri": "s3Uri"
        }
      },
      "ContentType": "content type",
      "CompressionType": "compression type",
      "SplitType": "split type"
    },
    "TransformOutput": {
      "S3OutputPath": "s3Uri",
      "Accept": "accept",
      "AssembleWith": "assemblyType",
      "KmsKeyId": "kmsKeyId"
    },
    "TransformResources": {
      "InstanceType": "instanceType",
      "InstanceCount": 3
    },
    "CreationTime": "2018-10-06T12:26:13Z",
    "TransformStartTime": "2018-10-06T12:26:13Z",
    "TransformEndTime": "2018-10-06T12:26:13Z",
    "Tags": {}
  }
}
```

## Endpoint state change<a name="eventbridge-endpoint"></a>

Indicates a change in the status of a SageMaker hosted real\-time inference endpoint\.

The following shows an event with an endpoint in the `IN_SERVICE` state\.

```
{
  "version": "0",
  "id": "d2921b5a-b0ad-cace-a8e3-0f159d018e06",
  "detail-type": "SageMaker Endpoint State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "1583831889050",
  "region": "us-west-2",
  "resources": [
      "arn:aws:sagemaker:us-west-2:123456789012:endpoint/myendpoint"
  ],
  "detail": {
      "EndpointName": "MyEndpoint",
      "EndpointArn": "arn:aws:sagemaker:us-west-2:123456789012:endpoint/myendpoint",
      "EndpointConfigName": "MyEndpointConfig",
      "ProductionVariants": [
          {
              "DesiredWeight": 1.0,
              "DesiredInstanceCount": 1.0
          }
      ],
      "EndpointStatus": "IN_SERVICE",
      "CreationTime": 1592411992203.0,
      "LastModifiedTime": 1592411994287.0,
      "Tags": {

      }
  }
}
```

## Feature group state change<a name="eventbridge-feature-group"></a>

Indicates a change either in the `FeatureGroupStatus` or the `OfflineStoreStatus` of a SageMaker feature group\.

```
{
  "version": "0",
  "id": "93201303-abdb-36a4-1b9b-4c1c3e3671c0",
  "detail-type": "SageMaker Feature Group State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2021-01-26T01:22:01Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:sagemaker:us-east-1:123456789012:feature-group/sample-feature-group"
  ],
  "detail": {
    "FeatureGroupArn": "arn:aws:sagemaker:us-east-1:123456789012:feature-group/sample-feature-group",
    "FeatureGroupName": "sample-feature-group",
    "RecordIdentifierFeatureName": "RecordIdentifier",
    "EventTimeFeatureName": "EventTime",
    "FeatureDefinitions": [
      {
        "FeatureName": "RecordIdentifier",
        "FeatureType": "Integral"
      },
      {
        "FeatureName": "EventTime",
        "FeatureType": "Fractional"
      }
    ],
    "CreationTime": 1611624059000,
    "OnlineStoreConfig": {
      "EnableOnlineStore": true
    },
    "OfflineStoreConfig": {
      "S3StorageConfig": {
        "S3Uri": "s3://offline/s3/uri"
      },
      "DisableGlueTableCreation": false,
      "DataCatalogConfig": {
        "TableName": "sample-feature-group-1611624059",
        "Catalog": "AwsDataCatalog",
        "Database": "sagemaker_featurestore"
      }
    },
    "RoleArn": "arn:aws:iam::123456789012:role/SageMakerRole",
    "FeatureGroupStatus": "Active",
    "Tags": {}
  }
}
```

## Model package state change<a name="eventbridge-model-package"></a>

Indicates a change in the status of a SageMaker model package\.

```
{
  "version": "0",
  "id": "844e2571-85d4-695f-b930-0153b71dcb42",
  "detail-type": "SageMaker Model Package State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2021-02-24T17:00:14Z",
  "region": "us-east-2",
  "resources": [
    "arn:aws:sagemaker:us-east-2:123456789012:model-package/versionedmp-p-idy6c3e1fiqj/2"
  ],
  "source": [
    "aws.sagemaker"
  ],
  "detail": {
    "ModelPackageGroupName": "versionedmp-p-idy6c3e1fiqj",
    "ModelPackageVersion": 2,
    "ModelPackageArn": "arn:aws:sagemaker:us-east-2:123456789012:model-package/versionedmp-p-idy6c3e1fiqj/2",
    "CreationTime": "2021-02-24T17:00:14Z",
    "InferenceSpecification": {
      "Containers": [
        {
          "Image": "257758044811.dkr.ecr.us-east-2.amazonaws.com/sagemaker-xgboost:1.0-1-cpu-py3",
          "ImageDigest": "sha256:4dc8a7e4a010a19bb9e0a6b063f355393f6e623603361bd8b105f554d4f0c004",
          "ModelDataUrl": "s3://sagemaker-project-p-idy6c3e1fiqj/versionedmp-p-idy6c3e1fiqj/AbaloneTrain/pipelines-4r83jejmhorv-TrainAbaloneModel-xw869y8C4a/output/model.tar.gz"
        }
      ],
      "SupportedContentTypes": [
        "text/csv"
      ],
      "SupportedResponseMIMETypes": [
        "text/csv"
      ]
    },
    "ModelPackageStatus": "Completed",
    "ModelPackageStatusDetails": {
      "ValidationStatuses": [],
      "ImageScanStatuses": []
    },
    "CertifyForMarketplace": false,
    "ModelApprovalStatus": "Rejected",
    "MetadataProperties": {
      "GeneratedBy": "arn:aws:sagemaker:us-east-2:123456789012:pipeline/versionedmp-p-idy6c3e1fiqj/execution/4r83jejmhorv"
    },
    "ModelMetrics": {
      "ModelQuality": {
        "Statistics": {
          "ContentType": "application/json",
          "S3Uri": "s3://sagemaker-project-p-idy6c3e1fiqj/versionedmp-p-idy6c3e1fiqj/script-2021-02-24-10-55-15-413/output/evaluation/evaluation.json"
        }
      }
    },
    "LastModifiedTime": "2021-02-24T17:00:14Z"
  }
}
```

## Pipeline execution state change<a name="eventbridge-pipeline"></a>

Indicates a change in the status of a SageMaker pipeline execution\.

```
{
  "version": "0",
  "id": "315c1398-40ff-a850-213b-158f73kd93ir",
  "detail-type": "SageMaker Model Building Pipeline Execution Status Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2021-03-15T16:10:11Z",
  "region": "us-east-1",
  "resources": ["arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123", "arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123/execution/p4jn9xou8a8s"],
  "detail": {
    "pipelineExecutionDisplayName": "SomeDisplayName",
    "currentPipelineExecutionStatus": "Succeeded",
    "previousPipelineExecutionStatus": "Executing",
    "executionStartTime": "2021-03-15T16:03:13Z",
    "executionEndTime": "2021-03-15T16:10:10Z",
    "pipelineExecutionDescription": "SomeDescription",
    "pipelineArn": "arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123",
    "pipelineExecutionArn": "arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123/execution/p4jn9xou8a8s"
  }
}
```

## Pipeline step state change<a name="eventbridge-pipeline-step"></a>

Indicates a change in the status of a SageMaker pipeline step\.

If there is a cache hit, the event contains the `cacheHitResult` field\.

If the value of `currentStepStatus` is `Failed`, the event contains the `failureReason` field, which provides a description of why the step failed\.

```
{
  "version": "0",
  "id": "ea37ccbb-5e2b-05e9-4073-1daazc940304",
  "detail-type": "SageMaker Model Building Pipeline Execution Step Status Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2021-03-15T16:10:10Z",
  "region": "us-east-1",
  "resources": ["arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123", "arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123/execution/p4jn9xou8a8s"],
  "detail": {
    "metadata": {
      "processingJob": {
        "arn": "arn:aws:sagemaker:us-east-1:123456789012:processing-job/pipelines-p4jn9xou8a8s-myprocessingstep1-tmgxry49ug"
      }
    },
    "stepStartTime": "2021-03-15T16:03:14Z",
    "stepEndTime": "2021-03-15T16:10:09Z",
    "stepName": "myprocessingstep1",
    "stepType": "Processing",
    "previousStepStatus": "Executing",
    "currentStepStatus": "Succeeded",
    "pipelineArn": "arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123",
    "pipelineExecutionArn": "arn:aws:sagemaker:us-east-1:123456789012:pipeline/myPipeline-123/execution/p4jn9xou8a8s"
  }
}
```

## SageMaker image state change<a name="eventbridge-image-state"></a>

Indicates a change in the status of a SageMaker image\.

```
{
  "version": "0",
  "id": "cee033a3-17d8-49f8-865f-b9ebf485d9ee",
  "detail-type": "SageMaker Image State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2021-04-29T01:29:59Z",
  "region": "us-east-1",
  "resources": ["arn:aws:sagemaker:us-west-2:123456789012:image/cee033a3-17d8-49f8-865f-b9ebf485d9ee"],
  "detail": {
    "ImageName": "cee033a3-17d8-49f8-865f-b9ebf485d9ee",
    "ImageArn": "arn:aws:sagemaker:us-west-2:123456789012:image/cee033a3-17d8-49f8-865f-b9ebf485d9ee",
    "ImageStatus": "Creating",
    "Version": 1.0,
    "Tags": {}
  }
}
```

## SageMaker image version state change<a name="eventbridge-image-version-state"></a>

Indicates a change in the status of a SageMaker image version\.

```
{
  "version": "0",
  "id": "07fc4615-ebd7-15fc-1746-243411f09f04",
  "detail-type": "SageMaker Image Version State Change",
  "source": "aws.sagemaker",
  "account": "123456789012",
  "time": "2021-04-29T01:29:59Z",
  "region": "us-east-1",
  "resources": ["arn:aws:sagemaker:us-west-2:123456789012:image-version/07800032-2d29-48b7-8f82-5129225b2a85"],
  "detail": {
    "ImageArn": "arn:aws:sagemaker:us-west-2:123456789012:image/a70ff896-c832-4fe8-add6-eba25a0f43e6",
    "ImageVersionArn": "arn:aws:sagemaker:us-west-2:123456789012:image-version/07800032-2d29-48b7-8f82-5129225b2a85",
    "ImageVersionStatus": "Creating",
    "Version": 1.0,
    "Tags": {}
  }
}
```

For more information about the status values and their meanings for SageMaker jobs, endpoints, and pipelines, see the following links:
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAlgorithm.html#sagemaker-DescribeAlgorithm-response-AlgorithmStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAlgorithm.html#sagemaker-DescribeAlgorithm-response-AlgorithmStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html#sagemaker-DescribeEndpoint-response-EndpointStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html#sagemaker-DescribeEndpoint-response-EndpointStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html#sagemaker-DescribeFeatureGroup-response-FeatureGroupStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html#sagemaker-DescribeFeatureGroup-response-FeatureGroupStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#sagemaker-DescribeLabelingJob-response-LabelingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#sagemaker-DescribeLabelingJob-response-LabelingJobStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeModelPackage.html#sagemaker-DescribeModelPackage-response-ModelPackageStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeModelPackage.html#sagemaker-DescribeModelPackage-response-ModelPackageStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeNotebookInstance.html#sagemaker-DescribeNotebookInstance-response-NotebookInstanceStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeNotebookInstance.html#sagemaker-DescribeNotebookInstance-response-NotebookInstanceStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribePipelineExecution.html#sagemaker-DescribePipelineExecution-response-PipelineExecutionStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribePipelineExecution.html#sagemaker-DescribePipelineExecution-response-PipelineExecutionStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_PipelineExecutionStep.html#sagemaker-Type-PipelineExecutionStep-StepStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_PipelineExecutionStep.html#sagemaker-Type-PipelineExecutionStep-StepStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html#sagemaker-DescribeProcessingJob-response-ProcessingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html#sagemaker-DescribeProcessingJob-response-ProcessingJobStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html#sagemaker-DescribeTrainingJob-response-TrainingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html#sagemaker-DescribeTrainingJob-response-TrainingJobStatus)
+ [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html#sagemaker-DescribeTransformJob-response-TransformJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html#sagemaker-DescribeTransformJob-response-TransformJobStatus)

For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)\.

## Endpoint deployment state change<a name="eventbridge-deployment-state"></a>

**Important**  
The following examples may not work for all endpoints\. For a list of features that may exclude your endpoint, see the [Exclusions](deployment-guardrails-exclusions.md) page\.

Indicates a state change for an endpoint deployment\. The following example shows an endpoint updating with a blue/green canary deployment\.

```
{
    "version": "0",
    "id": "0bd4a141-0a02-9d8a-f977-3924c3fb259c",
    "detail-type": "SageMaker Endpoint Deployment State Change",
    "source": "aws.sagemaker",
    "account": "123456789012",
    "time": "2021-10-25T01:52:12Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:sagemaker:us-west-2:651393343886:endpoint/sample-endpoint"
    ],
    "detail": {
        "EndpointName": "sample-endpoint",
        "EndpointArn": "arn:aws:sagemaker:us-west-2:651393343886:endpoint/sample-endpoint",
        "EndpointConfigName": "sample-endpoint-config-1",
        "ProductionVariants": [
            {
                "VariantName": "AllTraffic",
                "CurrentWeight": 1,
                "DesiredWeight": 1,
                "CurrentInstanceCount": 3,
                "DesiredInstanceCount": 3
            }
        ],
        "EndpointStatus": "UPDATING",
        "CreationTime": 1635195148181,
        "LastModifiedTime": 1635195148181,
        "Tags": {},
        "PendingDeploymentSummary": {
            "EndpointConfigName": "sample-endpoint-config-2",
            "StartTime": Timestamp,
            "ProductionVariants": [
                {
                    "VariantName": "AllTraffic",
                    "CurrentWeight": 1,
                    "DesiredWeight": 1,
                    "CurrentInstanceCount": 1,
                    "DesiredInstanceCount": 3,
                    "VariantStatus": [
                        {
                            "Status": "Baking",
                            "StatusMessage": "Baking for 600 seconds (TerminationWaitInSeconds) with traffic enabled on canary capacity of 1 instance(s).",
                            "StartTime": 1635195269181,
                        }
                    ]
                }
            ]
        }
    }
}
```

The following example indicates a state change for an endpoint deployment, which is being updated with new capacity on an existing endpoint configuration\.

```
{
    "version": "0",
    "id": "0bd4a141-0a02-9d8a-f977-3924c3fb259c",
    "detail-type": "SageMaker Endpoint Deployment State Change",
    "source": "aws.sagemaker",
    "account": "123456789012",
    "time": "2021-10-25T01:52:12Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:sagemaker:us-west-2:651393343886:endpoint/sample-endpoint"
    ],
    "detail": {
        "EndpointName": "sample-endpoint",
        "EndpointArn": "arn:aws:sagemaker:us-west-2:651393343886:endpoint/sample-endpoint",
        "EndpointConfigName": "sample-endpoint-config-1",
        "ProductionVariants": [
            {
                "VariantName": "AllTraffic",
                "CurrentWeight": 1,
                "DesiredWeight": 1,
                "CurrentInstanceCount": 3,
                "DesiredInstanceCount": 6,
                "VariantStatus": [
                    {
                        "Status": "Updating",
                        "StatusMessage": "Scaling out desired instance count to 6.",
                        "StartTime": 1635195269181,
                    }
                ]
            }
        ],
        "EndpointStatus": "UPDATING",
        "CreationTime": 1635195148181,
        "LastModifiedTime": 1635195148181,
        "Tags": {},
    }
```

The following secondary deployment statuses are also available for endpoints \(found in the `VariantStatus` object\.
+ `Creating`: creating instances for the production variant\.

  Example message: `"Launching X instance(s)."`
+ `Deleting`: terminating instances for the production variant\.

  Example message: `"Terminating X instance(s)."`
+ `Updating`: updating capacity for the production variant\.

  Example messages: `"Launching X instance(s)."`, `"Scaling out desired instance count to X."`
+ `ActivatingTraffic`: turning on traffic for the production variant\.

  Example message: `"Activating traffic on canary capacity of X instance(s)."`
+ `Baking`: waiting period to monitor the CloudWatch alarms in the auto\-rollback configuration\.

  Example message: `"Baking for X seconds (TerminationWaitInSeconds) with traffic enabled on full capacity of Y instance(s)."`

## Model card state change<a name="eventbridge-model-card-state"></a>

Indicates a change in the status of an Amazon SageMaker Model Card\. For more information about model cards, see [Amazon SageMaker Model Cards](model-cards.md)\.

```
{
    "version": "0",
    "id": "aa7a9c4f-2caa-4d04-a6de-e67227ba4302",
    "detail-type": "SageMaker Model Card State Change",
    "source": "aws.sagemaker",
    "account": "123456789012",
    "time": "2022-11-30T00:00:00Z",
    "region": "us-east-1",
    "resources": [
        "arn:aws:sagemaker:us-east-1:123456789012:model-card/example-card"
    ],
    "detail": {
        "ModelCardVersion": 2,
        "LastModifiedTime": "2022-12-03T00:09:44.893854735Z",
        "LastModifiedBy": {
            "DomainId": "us-east-1",
            "UserProfileArn": "arn:aws:sagemaker:us-east-1:123456789012:user-profile/user",
            "UserProfileName": "user"
        },
        "CreationTime": "2022-12-03T00:09:33.084Z",
        "CreatedBy": {
            "DomainId": "us-east-1",
            "UserProfileArn": "arn:aws:sagemaker:us-east-1:123456789012:user-profile/user",
            "UserProfileName": "user"
        },
        "ModelCardName": "example-card",
        "ModelId": "example-model",
        "ModelCardStatus": "Draft",
        "AccountId": "123456789012",
        "SecurityConfig": {}
    }
}
```