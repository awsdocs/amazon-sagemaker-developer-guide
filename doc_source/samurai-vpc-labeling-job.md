# Run an Amazon SageMaker Ground Truth Labeling Job in an Amazon Virtual Private Cloud<a name="samurai-vpc-labeling-job"></a>

Amazon SageMaker Ground Truth; supports the following:
+ You can use Amazon S3 bucket policies to control access to buckets from specific Amazon VPC endpoints, or specific VPCs\. If you launch a labeling job and your input data is located in an Amazon S3 bucket with access restricted to users in your VPC, you can add a bucket policy to also grant a Ground Truth endpoint permission to access the bucket\. To learn more, see [Allow Ground Truth to Access VPC Restricted Amazon S3 Buckets](#sms-vpc-permissions-s3)\.
+ You can launch an [automated data labeling job](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html) in your VPC\. You use a VPC configuration to specify VPC subnets and security groups\. SageMaker uses this configuration to launch the training and inference jobs used for automated data labeling in your VPC\. To learn more see [Create an Automated Data Labeling job in a VPC](#sms-vpc-permissions-automated-labeling)\.

You may want to use these options in any of the following ways:
+ You can use both of these methods to launch a labeling job using a VPC protected Amazon S3 bucket with automated data labeling enabled\.
+ You can launch a labeling job using any [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) using a VPC protected bucket\.
+ You can launch a [custom labeling workflow](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates.html) using a VPC protected bucket\. Ground Truth interacts with your pre\-annotation and post\-annotation Lambda functions using an [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/endpoint-services-overview.html) endpoint\.

We recommend you review [Prerequisites to Run a Ground Truth Labeling Job in a VPC](#sms-vpc-gt-prereq) before you create a labeling job in an Amazon VPC\.

## Prerequisites to Run a Ground Truth Labeling Job in a VPC<a name="sms-vpc-gt-prereq"></a>

Review the following before you create a Ground Truth labeling job in an Amazon VPC\. 
+ If you are a new user of Ground Truth, review [Getting started](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-getting-started.html) to learn how to create a labeling job\.
+ If your input data is located in a VPC protected Amazon S3 bucket, your workers must access the worker portal from your VPC\.
**Note**  
When you launch a labeling job in your VPC, you must use a private work team\. To learn more about creating a private work team, see [Use a Private Workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-private.html)\.
+ If you want to launch an automated data labeling job in your VPC, do the following:
  + Use the instructions in [Create an Amazon S3 VPC Endpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html#train-vpc-s3)\. Training and inference containers used in the automated data labeling workflow use this endpoint to communicate with your buckets in Amazon S3\.
  + Review [Automate Data Labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html) to learn more about this feature\. Note that automated data labeling is supported for the following [built\-in task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html): [Image Classification \(Single Label\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-image-classification.html), [Image Semantic Segmentation](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-semantic-segmentation.html), [Bounding Box](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-bounding-box.html), [Text Classification \(Single Label\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-text-classification.html)\. Streaming labeling jobs do not support automated data labeling\.
+ Review the [Ground Truth Security and Permissions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-security-general.html) section and ensure:
  + the user creating the labeling job has all necessary permissions
  + you have created an IAM execution role with required permissions\. If you do not require fine\-tuned permissions for your use case, we recommend you use the IAM managed policies described in [Grant General Permissions To Get Started Using Ground Truth](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-security-permission.html#sms-security-permissions-get-started)\.

## Allow Ground Truth to Access VPC Restricted Amazon S3 Buckets<a name="sms-vpc-permissions-s3"></a>

The following sections provide details about the permissions Ground Truth requires to launch labeling jobs using Amazon S3 buckets that have access restricted to your VPC and VPC endpoints\. To learn how to restrict access to an Amazon S3 bucket to a VPC, see [Controlling access from VPC endpoints with bucket policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies-vpc-endpoint.html) in the Amazon Simple Storage Service User Guide guide\. To learn how to add a policy to an S3 bucket, see [Adding a bucket policy using the Amazon S3 console](https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)\.

**Note**  
Modifying policies on existing buckets can cause `IN_PROGRESS` Ground Truth jobs to fail\. We recommend you start new jobs using a new bucket\. If you want to continue using the same bucket, you can  
wait for an `IN_PROGRESS` job to finish or
terminate them using console or AWS CLI\.

You can restrict Amazon S3 bucket access to users in your VPC using an [AWS PrivateLink](http://aws.amazon.com/privatelink/) endpoint\. For example, the following S3 bucket policy allows access to a specific bucket, `<bucket-name>`, from `<vpc>` and the endpoint `<vpc-endpoint>` only\. When you modify this policy, you must replace all *red\-italized text* with your resources and specifications\.

**Note**  
The following policy *denies* all entities *other than* users within a VPC to perform the actions listed in `Action`\. If you do not include actions in this list, they are still accessible to any entity that has access to this bucket and permission to perform those actions\. For example, if an IAM user has permission to perform `GetBucketLocation` on your Amazon S3 bucket, the policy below does not restrict the user from performing this action outside of the your VPC\.

```
{
  "Version": "2012-10-17",
  "Id": "Policy1415115909152",
  "Statement": [
    {
      "Sid": "Access-to-specific-VPCE-only",
      "Principal": "*",
      "Action": [
            "s3:GetObject",
            "s3:PutObject"
      ]
      "Effect": "Deny",
      "Resource": ["arn:aws:s3:::<bucket-name>",
                   "arn:aws:s3:::<bucket-name>/*"],
      "Condition": {a
        "StringNotEquals": {
      "aws:sourceVpce": [
        "<vpc-endpoint>",
        "<vpc>"
    ]}
      }
    }
  ]
}
```

Ground Truth must be able to perform the following S3 actions on the S3 buckets you use to configure the labeling job\.

```
"s3:AbortMultipartUpload",
"s3:GetObject",
"s3:PutObject",
"s3:ListBucket",
"s3:GetBucketLocation"
```

You can do this by adding a Ground Truth endpoint to the bucket policy like the one previously mentioned\. The following table includes Ground Truth service endpoints for each AWS Region\. Add an endpoint in the same [AWS Region](https://docs.aws.amazon.com/general/latest/gr/rande.html) you use to run your labeling job to your bucket policy\.


****  

| AWS Region | Ground Truth Endpoint | 
| --- | --- | 
| us\-east\-2 | vpce\-02569ba1c40aad0bc | 
| us\-east\-1 | vpce\-08408e335ebf95b40 | 
| us\-west\-2 | vpce\-0ea07aa498eb78469 | 
| ca\-central\-1 | vpce\-0d46ea4c9ff55e1b7 | 
| eu\-central\-1 | vpce\-0865e7194a099183d | 
| eu\-west\-2 | vpce\-0bccd56798f4c5df0 | 
| eu\-west\-1 | vpce\-0788e7ed8628e595d | 
| ap\-south\-1 | vpce\-0d7fcda14e1783f11 | 
| ap\-southeast\-2 | vpce\-0b7609e6f305a77d4 | 
| ap\-southeast\-1 | vpce\-0e7e67b32e9efed27 | 
| ap\-northeast\-2 | vpce\-007893f89e05f2bbf | 
| ap\-northeast\-1 | vpce\-0247996a1a1807dbd | 

For example, the following policy restricts `GetObject` and `PutObject` actions on 
+ an Amazon S3 bucket to users in a VPC \(``\)
+ a VPC endpoint \(`<vpc-endpoint>`\)
+ a Ground Truth service endpoint \(`<ground-truth-endpoint>`\)

```
{
    "Version": "2012-10-17",
    "Id": "1",
    "Statement": [
        {
            "Sid": "DenyAccessFromNonGTandCustomerVPC",
            "Effect": "Deny",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket-name>",
                "arn:aws:s3:::<bucket-name>/*"
            ],
            "Condition": {
                "ForAllValues:StringNotEquals": {
                    "aws:sourceVpce": [
                        "<vpc-endpoint>",
                        "<ground-truth-endpoint>"
                    ],
                    "aws:SourceVpc": "<vpc>"
                }
            }
        }
    ]
}
```

If you want an IAM user to have permission to launch a labeling job using the Ground Truth console, you must also add the IAM user's ARN to the bucket policy using the `aws:PrincipalArn` condition\. This user must also have permission to perform the following Amazon S3 actions on the bucket you use to launch the labeling job\.

```
"s3:GetObject",
"s3:PutObject",
"s3:ListBucket",
"s3:GetBucketCors",
"s3:PutBucketCors",
"s3:ListAllMyBuckets",
```

The following code is an example of a bucket policy that restricts permission to perform the actions listed in `Action` on the S3 bucket `<bucket-name>` to the following:
+ `<role-name>`
+ the VPC endpoints listed in `aws:sourceVpce`
+ users within the VPC named `<vpc>`

```
{
    "Version": "2012-10-17",
    "Id": "1",
    "Statement": [
        {
            "Sid": "DenyAccessFromNonGTandCustomerVPC",
            "Effect": "Deny",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket-name>/*",
                "arn:aws:s3:::<bucket-name>"
            ],
            "Condition": {
                "ForAllValues:StringNotEquals": {
                    "aws:sourceVpce": [
                        "<vpc-endpoint>",
                        "<ground-truth-endpoint>"
                    ],
                    "aws:PrincipalArn": "arn:aws:iam::<aws-account-id>:role/<role-name>",
                    "aws:SourceVpc": "<vpc>"
                }
            }
        }
    ]
}
```

**Note**  
The Amazon VPC interface endpoints and the protected Amazon S3 buckets you use for input and output data must be located in the same AWS Region you use to create the labeling job\.

After you have granted Ground Truth permission to access your Amazon S3 buckets, you can use one of the topics in [Create a Labeling Job](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-create-labeling-job.html) to launch a labeling job\. Specify the VPC restricted Amazon S3 buckets for your input and output data buckets\.

## Create an Automated Data Labeling job in a VPC<a name="sms-vpc-permissions-automated-labeling"></a>

To create an automated data labeling job using an Amazon VPC, you provide a VPC configuration using the Ground Truth console or API operation `CreateLabelingJob`\. SageMaker uses the subnets and security groups you provide to launch the training and inferences jobs used for automated labeling\. 

**Important**  
Before you launch an automated data labeling job with a VPC configuration, make sure you have created an Amazon S3 VPC endpoint using the VPC you want to use for the labeling job\. To learn how, see [Create an Amazon S3 VPC Endpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html#train-vpc-s3)\.  
Additionally, if you create an automated data labeling job using a VPC restricted Amazon S3 bucket, you must follow the instructions in [Allow Ground Truth to Access VPC Restricted Amazon S3 Buckets](#sms-vpc-permissions-s3) to give Ground Truth permission to access the bucket\.

Use the following procedures to learn how to add a VPC configuration to your labeling job request\.

**Add a VPC configuration to an automated data labeling job \(console\):**

1. Follow the instructions in [Create a Labeling Job \(Console\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-create-labeling-job-console.html) and complete each step in the procedure, up to step 15\.

1. In the **Workers** section, select the check box next to **Enable automated data labeling**\.

1. Maximize **VPC configuration** section of the console by selecting the arrow\.

1. Specify the **Virtual private cloud \(VPC\)** that you want to use for your automated data labeling job\.

1. Select the drop down menu under **Subnets** and select one or more subnets\.

1. Select the drop down menu under **Security groups** and select one or more groups\.

1. Complete all remaining steps of the procedure in [Create a Labeling Job \(Console\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-create-labeling-job-console.html)\.

**Add a VPC configuration to an automated data labeling job \(API\):**  
To configure a labeling job using the Ground Truth API operation, `CreateLabelingJob`, you can use the instructions in [Create an Automated Data Labeling Job \(API\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html#sms-create-automated-labeling-api) to configure your request\. In addition to the parameters described in this documentation, you must include a `VpcConfig` parameter in `LabelingJobResourceConfig` to specify one or more subnets and security groups using the following schema\.

```
"LabelingJobAlgorithmsConfig": { 
      "InitialActiveLearningModelArn": "string",
      "LabelingJobAlgorithmSpecificationArn": "string",
      "LabelingJobResourceConfig": { 
         "VolumeKmsKeyId": "string",
         "VpcConfig": { 
            "SecurityGroupIds": [ "string" ],
            "Subnets": [ "string" ]
         }
      }
}
```

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create an automated data labeling job in the US East \(N\. Virginia\) Region using a private workforce\. Replace all *red\-italized text* with your labeling job resources and specifications\. To learn more about the `CreateLabelingJob` operation, see the [Create a Labeling Job \(API\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-create-labeling-job-api.html) tutorial and [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) API documentation\.

```
import boto3
client = boto3.client(service_name='sagemaker')

response = client.create_labeling_job(
    LabelingJobName="example-labeling-job",
    LabelAttributeName="label",
    InputConfig={
        'DataSource': {
            'S3DataSource': {
                'ManifestS3Uri': "s3://bucket/path/manifest-with-input-data.json"
            }
        }
    },
    "LabelingJobAlgorithmsConfig": {
      "LabelingJobAlgorithmSpecificationArn": "arn:aws:sagemaker:us-east-1:027400017018:labeling-job-algorithm-specification/tasktype",
      "LabelingJobResourceConfig": { 
         "VpcConfig": { 
            "SecurityGroupIds": [ "sg-01233456789", "sg-987654321" ],
            "Subnets": [ "subnet-e0123456", "subnet-e7891011" ]
         }
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
        'TaskTitle': "Add task title here",
        'TaskDescription': "Add description of task here for workers",
        'NumberOfHumanWorkersPerDataObject': 1,
        'TaskTimeLimitInSeconds': 3600,
        'TaskAvailabilityLifetimeInSeconds': 21600,
        'MaxConcurrentTaskCount': 1000,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': "arn:aws:lambda:us-east-1:432418664414:function:ACS-tasktype"
        },
    Tags=[
        {
            'Key': "string",
            'Value': "string"
        },
    ]
)
```