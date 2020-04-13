# Create a Monitoring Schedule with an AWS CloudFormation Custom Resource<a name="model-monitor-cloudformation-monitoring-schedules"></a>

To use AWS CloudFormation to create a monitoring schedule, use an AWS CloudFormation custom resource\. The custom resource is in Python\. To deploy it, see [Python Lambda deployment](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html)\.

## Custom Resource<a name="model-monitor-cloudformation-custom-resource"></a>

Start by adding a custom resource to your AWS CloudFormation template\. This will point to a AWS Lambda function that you create next\. 

This resource allows you to customize the parameters for the monitoring schedule You can add or remove more parameters by modifying the AWS CloudFormation resource and the Lambda function in the following example resource\.

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "MonitoringSchedule": {
            "Type": "Custom::MonitoringSchedule",
            "Version": "1.0",
            "Properties": {
                "ServiceToken": "arn:aws:lambda:us-west-2:111111111111:function:lambda-name",
                "ScheduleName": "YourScheduleName",
                "EndpointName": "YourEndpointName",
                "BaselineConstraintsUri": "s3://your-baseline-constraints/constraints.json",
                "BaselineStatisticsUri": "s3://your-baseline-stats/statistics.json",
                "PostAnalyticsProcessorSourceUri": "s3://your-post-processor/postprocessor.py",
                "RecordPreprocessorSourceUri": "s3://your-preprocessor/preprocessor.py",
                "InputLocalPath": "/opt/ml/processing/endpointdata",
                "OutputLocalPath": "/opt/ml/processing/localpath",
                "OutputS3URI": "s3://your-output-uri",
                "ImageURI": "111111111111.dkr.ecr.us-west-2.amazonaws.com/your-image",
                "ScheduleExpression": "cron(0 * ? * * *)",
                "PassRoleArn": "arn:aws:iam::111111111111:role/AmazonSageMaker-ExecutionRole"
            }
        }
    }
}
```

## Lambda Custom Resource Code<a name="model-monitor-cloudformation-lambda-custom-resource-code"></a>

This AWS CloudFormation custom resource uses the [Custom Resource Helper](https://github.com/aws-cloudformation/custom-resource-helper) AWS library, which you can install with pip using `pip install crhelper`\. 

This Lambda function is invoked by AWS CloudFormation during the creation and deletion of the stack\. This Lambda function is responsible for creating and deleting the monitoring schedule and using the parameters defined in the custom resource described in the preceding section\.

```
import boto3
import botocore
import logging

from crhelper import CfnResource
from botocore.exceptions import ClientError


logger = logging.getLogger(__name__)
sm = boto3.client('sagemaker')

# cfnhelper makes it easier to implement a CloudFormation custom resource
helper = CfnResource()

# CFN Handlers

def handler(event, context):
    helper(event, context)


@helper.create
def create_handler(event, context):
    """
    Called when CloudFormation custom resource sends the create event
    """
    create_monitoring_schedule(event)


@helper.delete
def delete_handler(event, context):
    """
    Called when CloudFormation custom resource sends the delete event
    """
    schedule_name = get_schedule_name(event)
    delete_monitoring_schedule(schedule_name)


@helper.poll_create
def poll_create(event, context):
    """
    Return true if the resource has been created and false otherwise so
    CloudFormation polls again.
    """
    schedule_name = get_schedule_name(event)
    logger.info('Polling for creation of schedule: %s', schedule_name)
    return is_schedule_ready(schedule_name)

@helper.update
def noop():
    """
    Not currently implemented but crhelper will throw an error if it isn't added
    """
    pass

# Helper Functions

def get_schedule_name(event):
    return event['ResourceProperties']['ScheduleName']

def create_monitoring_schedule(event):
    schedule_name = get_schedule_name(event)
    monitoring_schedule_config = create_monitoring_schedule_config(event)

    logger.info('Creating monitoring schedule with name: %s', schedule_name)

    sm.create_monitoring_schedule(
        MonitoringScheduleName=schedule_name,
        MonitoringScheduleConfig=monitoring_schedule_config)

def is_schedule_ready(schedule_name):
    is_ready = False

    schedule = sm.describe_monitoring_schedule(MonitoringScheduleName=schedule_name)
    status = schedule['MonitoringScheduleStatus']

    if status == 'Scheduled':
        logger.info('Monitoring schedule (%s) is ready', schedule_name)
        is_ready = True
    elif status == 'Pending':
        logger.info('Monitoring schedule (%s) still creating, waiting and polling again...', schedule_name)
    else:
        raise Exception('Monitoring schedule ({}) has unexpected status: {}'.format(schedule_name, status))

    return is_ready

def create_monitoring_schedule_config(event):
    props = event['ResourceProperties']

    return {
        "ScheduleConfig": {
            "ScheduleExpression": props["ScheduleExpression"],
        },
        "MonitoringJobDefinition": {
            "BaselineConfig": {
                "ConstraintsResource": {
                    "S3Uri": props['BaselineConstraintsUri'],
                },
                "StatisticsResource": {
                    "S3Uri": props['BaselineStatisticsUri'],
                }
            },
            "MonitoringInputs": [
                {
                    "EndpointInput": {
                        "EndpointName": props["EndpointName"],
                        "LocalPath": props["InputLocalPath"],
                    }
                }
            ],
            "MonitoringOutputConfig": {
                "MonitoringOutputs": [
                    {
                        "S3Output": {
                            "S3Uri": props["OutputS3URI"],
                            "LocalPath": props["OutputLocalPath"],
                        }
                    }
                ],
            },
            "MonitoringResources": {
                "ClusterConfig": {
                    "InstanceCount": 1,
                    "InstanceType": "ml.t3.medium",
                    "VolumeSizeInGB": 50,
                }
            },
            "MonitoringAppSpecification": {
                "ImageUri": props["ImageURI"],
                "RecordPreprocessorSourceUri": props['PostAnalyticsProcessorSourceUri'],
                "PostAnalyticsProcessorSourceUri": props['PostAnalyticsProcessorSourceUri'],
            },
            "StoppingCondition": {
                "MaxRuntimeInSeconds": 300
            },
            "RoleArn": props["PassRoleArn"],
        }
    }


def delete_monitoring_schedule(schedule_name):
    logger.info('Deleting schedule: %s', schedule_name)
    try:
        sm.delete_monitoring_schedule(MonitoringScheduleName=schedule_name)
    except ClientError as e:
        if e.response['Error']['Code'] == 'ResourceNotFound':
            logger.info('Resource not found, nothing to delete')
        else:
            logger.error('Unexpected error while trying to delete monitoring schedule')
            raise e
```