# Custom Workflows via the API<a name="sms-custom-templates-step4"></a>

When you have created your custom UI template \(Step 2\) and processing Lambda functions \(Step 3\), you should place the template in an Amazon S3 bucket with a file name format of: `<FileName>.liquid.html`\.

Use the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) action to configure your task\. You'll use the location of a custom template \([Step 2: Creating your custom labeling task template](sms-custom-templates-step2.md)\) stored in a `<filename>.liquid.html` file on S3 as the value for the `UiTemplateS3Uri` field in the [ `UiConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html) object within the [ `HumanTaskConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html) object\.

For the AWS Lambda tasks described in [Step 3: Processing with AWS Lambda](sms-custom-templates-step3.md), the post\-annotation task's ARN will be used as the value for the `AnnotationConsolidationLambdaArn` field, and the pre\-annotation task will be used as the value for the `PreHumanTaskLambdaArn.` 