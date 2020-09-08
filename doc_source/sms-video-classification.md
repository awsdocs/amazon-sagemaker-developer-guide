# Video Classification<a name="sms-video-classification"></a>

Use an Amazon SageMaker Ground Truth video classification labeling task when you need workers to classify videos using predefined labels that you specify\. Workers are shown videos and are asked to choose one label for each video\.

You create a video classification labeling job using the Ground Truth section of the Amazon SageMaker console or the [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. 

**Important**  
For this task type, if you create your own manifest file, use `"source-ref"` to identify the location of each video file in Amazon S3 that you want labeled\. For more information, see [Input Data](sms-data-input.md)\.

## Create a Video Classification Labeling Job \(Console\)<a name="sms-creating-video-classification-console"></a>

You can follow the instructions in [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a video classification labeling job in the SageMaker console\. In step 10, choose **Video** from the **Task category** dropdown list, and choose **Video Classification** as the task type\. 

Ground Truth provides a worker UI similar to the following for labeling tasks\. When you create a labeling job in the console, you specify instructions to help workers complete the job and labels from which workers can choose\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/vid_classification.gif)

## Create a Video Classification Labeling Job \(API\)<a name="sms-creating-video-classification-api"></a>

This section covers details you need to know when you create a labeling job using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.

Follow the instructions on [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) and do the following while you configure your request: 
+ Use a pre\-annotation Lambda function that ends with `PRE-VideoClassification`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ Use an annotation\-consolidation Lambda function that ends with `ACS-VideoClassification`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-video-classification-labeling-job,
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
            'UiTemplateS3Uri': 's3://bucket/path/worker-task-template.html'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-VideoClassification',
        'TaskKeywords': [
            'Video Classification',
        ],
        'TaskTitle': 'Video classification task',
        'TaskDescription': 'Select a label to classify this video',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-VideoClassification'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

### Provide a Template for Video Classification<a name="sms-custom-template-video-classification"></a>

If you create a labeling job using the API, you must supply a worker task template in `UiTemplateS3Uri`\. Copy and modify the following template by modifying the `short-instructions`, `full-instructions`, and `header`\. Upload this template to Amazon S3, and provide the Amazon S3 URI to this file in `UiTemplateS3Uri`\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

              <crowd-form>
                  <crowd-classifier
                    name="crowd-classifier"
                    categories="{{ task.input.labels | to_json | escape }}"
                    header="Please classify video"
                  >
                    <classification-target>
                       <video width="100%" controls/>
                        <source src="{{ task.input.taskObject | grant_read_access }}" type="video/mp4"/>
                        <source src="{{ task.input.taskObject | grant_read_access }}" type="video/webm"/>
                        <source src="{{ task.input.taskObject | grant_read_access }}" type="video/ogg"/>
                      Your browser does not support the video tag.
                      </video>
                    </classification-target>
                    <full-instructions header="Video classification instructions">
                      <ol><li><strong>Read</strong> the task carefully and inspect the video.</li>
                        <li><strong>Read</strong> the options and review the examples provided to understand more about the labels.</li>
                        <li><strong>Choose</strong> the appropriate label that best suits the video.</li></ol>
                    </full-instructions>
                    <short-instructions>
                      <h3><span style="color: rgb(0, 138, 0);">Good example</span></h3>
                        <p>Enter description to explain the correct label to the workers</p>
                        <p><img src="https://d7evko5405gb7.cloudfront.net/fe4fed9b-660c-4477-9294-2c66a15d6bbe/src/images/quick-instructions-example-placeholder.png" style="max-width:100%"></p>
                        <h3><span style="color: rgb(230, 0, 0);">Bad example</span></h3>
                        <p>Enter description of an incorrect label</p>
                        <p><img src="https://d7evko5405gb7.cloudfront.net/fe4fed9b-660c-4477-9294-2c66a15d6bbe/src/images/quick-instructions-example-placeholder.png" style="max-width:100%"></p>
                    </short-instructions>
                  </crowd-classifier>
              </crowd-form>
```

## Video Classification Output Data<a name="sms-vido-classification-output-data"></a>

Once you have created a video classification labeling job, your output data is located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 

To see an example of output manifest files for video classification labeling jobs, see [Classification Job Output](sms-data-output.md#sms-output-class)\.