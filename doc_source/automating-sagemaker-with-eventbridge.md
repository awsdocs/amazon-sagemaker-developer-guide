# Automating Amazon SageMaker with Amazon EventBridge<a name="automating-sagemaker-with-eventbridge"></a>

Amazon EventBridge monitors status change events in Amazon SageMaker labeling, training, hyperparameter tuning, process jobs, inference endpoints, and feature groups\. EventBridge enables you to automate SageMaker and respond automatically to events such as training job or endpoint status changes\. Events from SageMaker are delivered to EventBridge in near real time\. You can write simple rules to indicate which events are of interest to you, and what automated actions to take when an event matches a rule\. The actions that can be automatically triggered include the following:
+ Invoking an AWS Lambda function
+ Invoking Amazon EC2 Run Command
+ Relaying the event to Amazon Kinesis Data Streams
+ Activating an AWS Step Functions state machine
+ Notifying an Amazon SNS topic or an AWS SMS queue

Some examples of SageMaker status change events that EventBridge monitors include:
+ SageMaker training job state change

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
+ SageMaker HyperParameter tuning job state change

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
+ SageMaker transform job state change

  Indicates a change in the status of a SageMaker batch transform job\.

  If the value of `TransformJobStatus` is `Failed`, the event contains the `FailureReason` field, which provides a description of why the training job failed\.

  ```
  {
      "version": "0",
      "id": "844e2571-85d4-695f-b930-0153b71dcb42",
      "detail-type": "SageMaker Transform Job State Change",
      "source": "aws.sagemaker",
      "account": "123456789012",
      "time": "1583831889050",
      "region": "us-east-1",
      "resources": [
          "arn:aws:sagemaker:us-east-1:123456789012:transform-job/myjob"
      ],
      "detail": {
          "TransformJobName": "4b52bd8f-e034-4345-818d-884bdd7c9724",
          "TransformJobArn": "arn:aws:sagemaker:us-east-1:123456789012:transform-job/myjob",
          "TransformJobStatus": "Completed",
          "ModelName": "ModelName",
          "MaxConcurrentTransforms": 5,
          "MaxPayloadInMB": 10,
          "BatchStrategy": "Strategy",
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
          "CreationTime": "1583831889050",
          "TransformStartTime": "1583831889050",
          "TransformEndTime": "1583831889050",
          "Tags": {
  
          }
      }
  }
  ```
+ SageMaker HyperParameter tuning job state change

  Indicates a change in the status of a SageMaker hyperparameter tuning job\.

  ```
  {
    "version": "0",
    "id": "844e2571-85d4-695f-b930-0153b71dcb42",
    "detail-type": "SageMaker HyperParameter Tuning Job State Change",
    "source": "aws.sagemaker",
    "account": "123456789012",
    "time": "1583831889050",
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
+ SageMaker endpoint status change

  Indicates a change in the status of a SageMaker hosted real\-time inference endpoint\.

  The following shows an event with an endpoint in the `IN_SERVICE` state\.

  ```
  {
      'version': '0',
      'id': 'd2921b5a-b0ad-cace-a8e3-0f159d018e06',
      'detail-type': 'SageMakerEndpointStateChange',
      'source': 'aws.sagemaker',
      'account': '123456789012',
      'time': '1583831889050',
      'region': 'us-west-2',
      'resources': [
          'arn:aws:sagemaker:us-west-2:123456789012:endpoint/myendpoint'
      ],
      'detail': {
          'EndpointName': 'MyEndpoint',
          'EndpointArn': 'arn:aws:sagemaker:us-west-2:123456789012:endpoint/myendpoint',
          'EndpointConfigName': 'MyEndpointConfig',
          'ProductionVariants': [
              {
                  'DesiredWeight': 1.0,
                  'DesiredInstanceCount': 1.0
              }
          ],
          'EndpointStatus': 'IN_SERVICE',
          'CreationTime': 1592411992203.0,
          'LastModifiedTime': 1592411994287.0,
          'Tags': {
  
          }
      }
  }
  ```
+ SageMaker Feature Group State Change

  Indicates a change either in the `FeatureGroupStatus` or the `OfflineStoreStatus` of a SageMaker Feature Group\.

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

For more information about the status values and their meanings for SageMaker jobs and endpoints, see the following links:
+ [ `AlgorithmStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAlgorithm.html#sagemaker-DescribeAlgorithm-response-AlgorithmStatus)
+ [ `EndpointStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html#sagemaker-DescribeEndpoint-response-EndpointStatus)
+ [ `HyperParameterTuningJobStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html#sagemaker-DescribeHyperParameterTuningJob-response-HyperParameterTuningJobStatus)
+ [ `LabelingJobStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#sagemaker-DescribeLabelingJob-response-LabelingJobStatus)
+ [ `ModelPackageStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeModelPackage.html#sagemaker-DescribeModelPackage-response-ModelPackageStatus)
+ [ `NotebookInstanceStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeNotebookInstance.html#sagemaker-DescribeNotebookInstance-response-NotebookInstanceStatus)
+ [ `ProcessingJobStatus` ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html#sagemaker-DescribeProcessingJob-response-ProcessingJobStatus)
+ [ `TrainingJobStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html#sagemaker-DescribeTrainingJob-response-TrainingJobStatus)
+ [ `TransformJobStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html#sagemaker-DescribeTransformJob-response-TransformJobStatus)
+ [ `EndpointStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html#sagemaker-DescribeEndpoint-response-EndpointStatus)
+ [ `FeatureGroupStatus`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html#sagemaker-DescribeFeatureGroup-response-FeatureGroupStatus)

For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)\.