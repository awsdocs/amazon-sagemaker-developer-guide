# Create a Labeling Job \(API\)<a name="sms-create-labeling-job-api"></a>

To create a labeling job using the Amazon SageMaker API, you use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. For specific instructions on creating a labeling job for a built\-in task type, see that [task type page](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html)\. 

To use this operation, you need the following:
+ A worker task template \(`UiTemplateS3Uri`\) or human task UI ARN \(`HumanTaskUiArn`\) in Amazon S3\. 
  + For 3D point cloud labeling jobs, use the ARN listed in `[HumanTaskUiArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-HumanTaskUiArn)` for your task type\. 
  + If you are using a built\-in task type other than 3D point cloud tasks, you can add your worker instructions to one of the pre\-built templates and save the template \(using a \.html or \.liquid extension\) in your S3 bucket\. Find the pre\-build templates on your [task type page](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html)\.
  + If you are using a custom labeling workflow, you can create a custom template and save the template in your S3 bucket\. To learn how to built a custom worker template, see [Step 2: Creating your custom labeling task template](sms-custom-templates-step2.md)\. For custom HTML elements that you can use to customize your template, see [Crowd HTML Elements Reference](sms-ui-template-reference.md)\. For a repository of demo templates for a variety of labeling tasks, see [Amazon SageMaker Ground Truth Sample Task UIs ](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis)\.
+ At least one S3 bucket to store your input and output data\. 
+ An input manifest file that specifies your input data\. For information about creating an input manifest, see [Input Data](sms-data-input.md)\. 
+ A label category configuration file\. For 3D point cloud task type, use the format in [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md)\. For all other built\-in task types and custom tasks, your label category configuration file must be a JSON file in the following format\. Identify the labels you want to use by replacing `label_1`, `label_2`,`...`,`label_n` with your label categories\.

  ```
  {
      "document-version": "2018-11-28"
      "labels": [
          {"label": "label_1"},
          {"label": "label_2"},
          ...
          {"label": "label_n"}
      ]
  }
  ```
+ An AWS Identity and Access Management \(IAM\) role with the AmazonSageMakerFullAccess IAM policy attached and with permissions to access your S3 buckets\. If you require more granular permissions, see [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)\. Note that AmazonSageMakerFullAccess will grant your role permission to access all S3 buckets with `sagemaker` in the name\. 

  If your input or output bucket name does not contain `sagemaker`, you can attach a policy similar to the following to the role that is passed to the `CreateLabelingJob` operation\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "s3:GetObject"
              ],
              "Resource": [
                  "arn:aws:s3:::my_input_bucket/*"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject"
              ],
              "Resource": [
                  "arn:aws:s3:::my_output_bucket/*"
              ]
          }
      ]
  }
  ```
+ A pre\-annotation and post\-annotation \(or annotation\-consolidation\) AWS Lambda function Amazon Resource Name \(ARN\) to process your input and output data\. 
  + Lambda functions are predefined in each AWS Region for built\-in task types\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 
  + For custom labeling workflows, you must provide a custom pre\- and post\-annotation Lambda ARN\. To learn how to create these Lambda functions, see [Step 3: Processing with AWS Lambda](sms-custom-templates-step3.md)\.
+ A work team ARN\. To learn more about work teams and workforces, see [Create and Manage Workforces](sms-workforce-management.md)\. 

  If you use the [Amazon Mechanical Turk workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-public.html), use the `ContentClassifiers` parameter in `CreateLabelingJob` to declare that your content is free of personally identifiable information or adult content\. If your content contains personally identifiable information or adult content, SageMaker might restrict the Amazon Mechanical Turk workers that can view your task\. 

  If you are creating a labeling job for a point cloud task type, you cannot use the Amazon Mechanical Turk workforce\. 
+ \(Optional\) For [some task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-annotation-consolidation.html), you can have multiple workers label a single data object by inputting a number greater than one for the `NumberOfHumanWorkersPerDataObject` parameter\. For more information about annotation consolidation, see [Consolidate Annotations ](sms-annotation-consolidation.md)\.

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job for a built\-in task type in the US East \(N\. Virginia\) Region\. 

------
#### [ AWS SDK for Python \(Boto3\) ]

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job for a built\-in task type in the US East \(N\. Virginia\) Region using a private workforce\. Replace all *red\-italized text* with your labeling job resources and specifications\. 

```
response = client.create_labeling_job(
    LabelingJobName="example-labeling-job",
    LabelAttributeName="label",
    InputConfig={
        'DataSource': {
            'S3DataSource': {
                'ManifestS3Uri': "s3://bucket/path/manifest-with-input-data.json"
            }
        },
        'DataAttributes': {
            'ContentClassifiers': [
                "FreeOfPersonallyIdentifiableInformation"|"FreeOfAdultContent",
            ]
        }
    },
    OutputConfig={
        'S3OutputPath': "s3://bucket/path/file-to-store-output-data",
        'KmsKeyId': "string"
    },
    RoleArn="arn:aws:iam::*:role/*,
    LabelCategoryConfigS3Uri="s3://bucket/path/label-categories.json",
    StoppingConditions={
        'MaxHumanLabeledObjectCount': 123,
        'MaxPercentageOfInputDatasetLabeled': 123
    },
    HumanTaskConfig={
        'WorkteamArn': "arn:aws:sagemaker:region:*:workteam/private-crowd/*",
        'UiConfig': {
            'UiTemplateS3Uri': "s3://bucket/path/custom-worker-task-template.html"
        },
        'PreHumanTaskLambdaArn': "arn:aws:lambda:us-east-1:432418664414:function:PRE-tasktype",
        'TaskKeywords': [
            "Images",
            "Classification",
            "Multi-label"
        ],
        'TaskTitle': "Multi-label image classification task",
        'TaskDescription': "Select all labels that apply to the images shown",
        'NumberOfHumanWorkersPerDataObject': 1,
        'TaskTimeLimitInSeconds': 3600,
        'TaskAvailabilityLifetimeInSeconds': 21600,
        'MaxConcurrentTaskCount': 1000,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': "arn:aws:lambda:us-east-1:432418664414:function:ACS-"
        },
    Tags=[
        {
            'Key': "string",
            'Value': "string"
        },
    ]
)
```

------
#### [ AWS CLI ]

The following is an example of an AWS CLI request to create a labeling job for a built\-in task type in the US East \(N\. Virginia\) Region using the [Amazon Mechanical Turk workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-public.html)\. For more information, see [start\-human\-loop](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/create-labeling-job.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. Replace all *red\-italized text* with your labeling job resources and specifications\. 

```
$ aws --region us-east-1 sagemaker create-labeling-job \
--labeling-job-name "example-labeling-job" \
--label-attribute-name "label" \
--role-arn "arn:aws:iam::account-id:role/role-name" \
--input-config '{
        "DataAttributes": {
            "ContentClassifiers": [
                "FreeOfPersonallyIdentifiableInformation",
                "FreeOfAdultContent"
            ]
        },
        "DataSource": {
            "S3DataSource": {
                "ManifestS3Uri": "s3://bucket/path/manifest-with-input-data.json"
            }
        }
    }' \
--output-config '{
        "KmsKeyId": "",
        "S3OutputPath": "s3://bucket/path/file-to-store-output-data"
    }' \
--human-task-config '{
        "AnnotationConsolidationConfig": {
            "AnnotationConsolidationLambdaArn": "arn:aws:lambda:us-east-1:432418664414:function:ACS-"
        },
        "TaskAvailabilityLifetimeInSeconds": 21600,
        "TaskTimeLimitInSeconds": 3600,
        "NumberOfHumanWorkersPerDataObject": 1,
        "PreHumanTaskLambdaArn":  "arn:aws:lambda:us-east-1:432418664414:function:PRE-tasktype",
        "WorkteamArn": "arn:aws:sagemaker:us-east-1:394669845002:workteam/public-crowd/default",
        "PublicWorkforceTaskPrice": {
            "AmountInUsd": {
                "Dollars": 0,
                "TenthFractionsOfACent": 6,
                "Cents": 3
            }
        },
        "TaskDescription": "Select all labels that apply to the images shown",
        "MaxConcurrentTaskCount": 1000,
        "TaskTitle": "Multi-label image classification task",,
        "TaskKeywords": [
            "Images",
            "Classification",
            "Multi-label"
        ],
        "UiConfig": {
            "UiTemplateS3Uri": "s3://bucket/path/custom-worker-task-template.html"
        }
    }'
```

------

**Important**  
If you set `TaskTimeLimitInSeconds` to be greater than one hour \(3,600 seconds\), you must increases the max session duration of your execution role to be greater than or equal to the task timeout\.   
You can modify the max session duration of your execution rule using the IAM console, AWS CLI, and IAM API\. To modify your execution role, go to [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, select your preferred method \(console, CLI, or API\) to modify the role from the **Topics** list, and then select **Modifying a Role Maximum Session Duration** to view the instructions\. For 3D point cloud task types, refer to [Increase MaxSessionDuration for Execution Role](sms-point-cloud-general-information.md#sms-3d-pointcloud-maxsessduration)\.

For more information about this operation, see [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html)\. For information about how to use other language\-specific SDKs, see [See Also](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_SeeAlso) in the `CreateLabelingJobs` topic\. 