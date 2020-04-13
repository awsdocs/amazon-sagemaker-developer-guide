# Image Classification \(Multi\-label\)<a name="sms-image-classification-multilabel"></a>

Use an Amazon SageMaker Ground Truth multi\-label image classification labeling task when you need workers to classify multiple objects in an image\. For example, the following image features a dog and a cat\. You can use multi\-label image classification to associate the labels "dog" and "cat" with this image\.

![\[Photo by Anusha Barwa on Unsplash\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/dog-cat-photo.jpg)

When working on a multi\-label image classification task, workers should choose all applicable labels, but must choose at least one\. When creating a job using this task type, you can provide up to 50 label\-categories\. 

When creating a labeling job in the console, Amazon SageMaker Ground Truth doesn't provide a "none" category for when none of the labels applies to an image\. To provide this option to workers, include a label similar to "none" or "other" when you create a multi\-label image classification job\. 

When you create a multi\-label image classification job in the console, you will be provided with a default worker UI template that you can use to:
+ Specify the labels that you want the worker to see 
+ Create worker instructions

See [Create a Multi\-Label Image Classification Labeling Job \(Console\)](#sms-creating-multilabel-image-classification-console) for an example of the template provided in the console\. 

When you create a multi\-label image classification job using the Amazon SageMaker API or your preferred Amazon SageMaker SDK, you will need to provide a custom worker task template which will be used to generated your workers' UI\. For more information, see [Create a Custom Template for Multi\-label Image Classification](#custom-template-multi-image-label-classification)\.

To restrict workers to choosing a single label for each image, use the [Image Classification](sms-image-classification.md) task type\.

**Note**  
Automated labeling \(auto\-labeling\) isn't supported for the multi\-label image classification task type\. 

**Topics**
+ [Create a Multi\-Label Image Classification Labeling Job \(Console\)](#sms-creating-multilabel-image-classification-console)
+ [Create a Multi\-Label Image Classification Labeling Job \(API\)](#sms-create-multi-select-image-classification-job-console)
+ [Multi\-label Image Classification Output Data](#sms-image-classification-multi-output-data)

## Create a Multi\-Label Image Classification Labeling Job \(Console\)<a name="sms-creating-multilabel-image-classification-console"></a>

To create a multi\-label image classification labeling job, use the Ground Truth section of the Amazon SageMaker console\. Ground Truth provides a worker console similar to the following for labeling tasks\. When you create the labeling job with the console, you can modify the images and content that are shown\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/image-classification-multilabel-example.png)

To create a multi\-label image classification labeling job, use the instructions in the [Getting started](sms-getting-started.md) guide\. When following [Step 2: Create a Labeling Job](sms-getting-started-step2.md), choose **Image classification \(Multi\-label\)** for **Task type**\.

## Create a Multi\-Label Image Classification Labeling Job \(API\)<a name="sms-create-multi-select-image-classification-job-console"></a>

To create a multi\-image label classification labeling job using the Amazon SageMaker API, you use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. To use this operation, you need:
+ A custom worker task template\. For information, see [Create a Custom Template for Multi\-label Image Classification](#custom-template-multi-image-label-classification)\.
+ At least one Amazon Simple Storage Service \(Amazon S3\) bucket to store your input and output data\. 
+ An input manifest file that specifies your input data\. For information about creating an input manifest, see [Input Data](sms-data-input.md)\. 
+ An AWS Identity and Access Management \(IAM\) role with the AmazonSageMakerFullAccess IAM policy attached and with permissions to access your S3 buckets\. For example, if the images specified in your manifest file are in an S3 bucket named `my_input_bucket`, and if you want the image\-labeling data to be stored in a bucket named `my_output_bucket`, you would attach the following IAM policy to the role that is passed to the `CreateLabelingJob` operation\.

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
+ A pre\-annotation AWS Lambda function ARN to process your input data\. Lambda functions are predefined in each AWS Region for multi\-label image classification labeling jobs\. Pre\-annotation Lambda functions for this task type end with `PRE-ImageMultiClassMultiLabel`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ A work team Amazon Resource Name \(ARN\)\. To learn more about work teams and workforces, see [Create and Manage Workforces](sms-workforce-management.md)\. 

  If you use the Amazon Mechanical Turk public workforce, use the `ContentClassifiers` parameter in `CreateLabelingJob` to declare that your content is free of personally identifiable information or adult content\. If your content contains personally identifiable information or adult content, Amazon SageMaker might restrict the Amazon Mechanical Turk workers that can view your task\.
+ \(Optional\) To have multiple workers label a single image \(by inputting a number greater than one for the `NumberOfHumanWorkersPerDataObject` parameter\), include an annotation\-consolidation Lambda ARN as an input to the `AnnotationConsolidationLambdaArn` parameter\. These Lambda functions are predefined in each Region for multi\-label image classification labeling jobs\. Annotation\-consolidation Lambda functions for this task type end with `ACS-ImageMultiClassMultiLabel`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-multi-label-image-classification-labeling-job,
    LabelAttributeName='label',
    InputConfig={
        'DataSource': {
            'S3DataSource': {
                'ManifestS3Uri': 's3://bucket/path/manifest-with-input-data.json'
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
        'KmsKeyId': 'string'
    },
    RoleArn='arn:aws:iam::*:role/*,
    LabelCategoryConfigS3Uri='s3://bucket/path/label-categories.json',
    StoppingConditions={
        'MaxHumanLabeledObjectCount': 123,
        'MaxPercentageOfInputDatasetLabeled': 123
    },
    HumanTaskConfig={
        'WorkteamArn': 'arn:aws:sagemaker:region:*:workteam/private-crowd/*',
        'UiConfig': {
            'UiTemplateS3Uri': 's3://bucket/path/custom-worker-task-template.html'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-ImageMultiClassMultiLabel',
        'TaskKeywords': [
            'Image Classification',
        ],
        'TaskTitle': 'Multi-label image classification task',
        'TaskDescription': 'Select all labels that apply to the images shown',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-ImageMultiClassMultiLabel'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

For more information about this operation, see [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html)\. For information about how to use other language\-specific SDKs, see [See Also](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_SeeAlso) in the `CreateLabelingJobs` topic\. 

### Create a Custom Template for Multi\-label Image Classification<a name="custom-template-multi-image-label-classification"></a>

If you create a labeling job using the API, you must create and supply a custom worker task template\. You can use the Amazon SageMaker [crowd\-image\-classifier\-multi\-select](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-ui-template-crowd-image-classifier-multi.html) HTML crowd element to create a template for a multi\-label image classification job\. The following example shows how to use this crowd element\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-image-classifier-multi-select
    name="animals"
    categories="['Cat', 'Dog', 'Horse', 'Pig', 'Bird']"
    src="https://images.unsplash.com/photo-1509205477838-a534e43a849f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1998&q=80"
    header="Please identify the animals in this image"
  >
    <full-instructions header="Classification Instructions">
      <p>If more than one label applies to the image, select multiple labels.</p>
      <p>If no labels apply, select <b>None of the above</b></p>
    </full-instructions>

    <short-instructions>
      <p>Read the task carefully and inspect the image.</p>
      <p>Choose the appropriate label(s) that best suit the image.</p>
    </short-instructions>
  </crowd-image-classifier-multi-select>
</crowd-form>
```

To learn how to create a custom template, see [Creating Custom Labeling Workflows](sms-custom-templates.md)\. 

## Multi\-label Image Classification Output Data<a name="sms-image-classification-multi-output-data"></a>

Once you have created a multi\-label image classification labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 

To see an example of output manifest files for multi\-label image classification labeling job, see [Multi\-label Classification Job Output](sms-data-output.md#sms-output-multi-label-classification)\.