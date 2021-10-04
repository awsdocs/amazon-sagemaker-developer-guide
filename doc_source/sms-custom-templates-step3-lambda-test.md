# Test Pre\-Annotation and Post\-Annotation Lambda Functions<a name="sms-custom-templates-step3-lambda-test"></a>

You can test your pre\-annotation and post annotation Lambda functions in the Lambda console\. If you are a new user of Lambda, you can learn how to test, or *invoke*, your Lambda functions in the console using the [Create a Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html#gettingstarted-zip-function) tutorial with the console in the AWS Lambda Developer Guide\.

You can use the sections on this page to learn how to test the Ground Truth pre\-annotation and post\-annotation templates provided through an AWS Serverless Application Repository \(SAR\)\. 

**Topics**
+ [Prerequisites](#sms-custom-templates-step3-lambda-test-pre)
+ [Test the Pre\-annotation Lambda Function](#sms-custom-templates-step3-lambda-test-pre-annotation)
+ [Test the Post\-Annotation Lambda Function](#sms-custom-templates-step3-lambda-test-post-annotation)

## Prerequisites<a name="sms-custom-templates-step3-lambda-test-pre"></a>

You must do the following to use the tests described on this page\.
+ You need access to the Lambda console, and you need permission to create and invoke Lambda functions\. To learn how to set up these permissions, see [Grant Permission to Create and Select an AWS Lambda Function](sms-custom-templates-step3-lambda-permissions.md#sms-custom-templates-step3-postlambda-create-perms)\.
+ If you have not deployed the Ground Truth SAR recipe, use the procedure in [Create Lambda Functions for a Custom Labeling Workflow](sms-custom-templates-step3-lambda-create.md) to do so\.
+ To test the post\-annotation Lambda function, you must have a data file in Amazon S3 with sample annotation data\. For a simple test, you can copy and paste the following code into a file and save it as `sample-annotations.json` and [upload this file to Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html)\. Note the S3 URI of this file—you need this information to configure the post\-annotation Lambda test\.

  ```
  [{"datasetObjectId":"0","dataObject":{"content":"To train a machine learning model, you need a large, high-quality, labeled dataset. Ground Truth helps you build high-quality training datasets for your machine learning models."},"annotations":[{"workerId":"private.us-west-2.0123456789","annotationData":{"content":"{\"crowd-entity-annotation\":{\"entities\":[{\"endOffset\":8,\"label\":\"verb\",\"startOffset\":3},{\"endOffset\":27,\"label\":\"adjective\",\"startOffset\":11},{\"endOffset\":33,\"label\":\"object\",\"startOffset\":28},{\"endOffset\":51,\"label\":\"adjective\",\"startOffset\":46},{\"endOffset\":65,\"label\":\"adjective\",\"startOffset\":53},{\"endOffset\":74,\"label\":\"adjective\",\"startOffset\":67},{\"endOffset\":82,\"label\":\"adjective\",\"startOffset\":75},{\"endOffset\":102,\"label\":\"verb\",\"startOffset\":97},{\"endOffset\":112,\"label\":\"verb\",\"startOffset\":107},{\"endOffset\":125,\"label\":\"adjective\",\"startOffset\":113},{\"endOffset\":134,\"label\":\"adjective\",\"startOffset\":126},{\"endOffset\":143,\"label\":\"object\",\"startOffset\":135},{\"endOffset\":169,\"label\":\"adjective\",\"startOffset\":153},{\"endOffset\":176,\"label\":\"object\",\"startOffset\":170}]}}"}}]},{"datasetObjectId":"1","dataObject":{"content":"Sift 3 cups of flour into the bowl."},"annotations":[{"workerId":"private.us-west-2.0123456789","annotationData":{"content":"{\"crowd-entity-annotation\":{\"entities\":[{\"endOffset\":4,\"label\":\"verb\",\"startOffset\":0},{\"endOffset\":6,\"label\":\"number\",\"startOffset\":5},{\"endOffset\":20,\"label\":\"object\",\"startOffset\":15},{\"endOffset\":34,\"label\":\"object\",\"startOffset\":30}]}}"}}]},{"datasetObjectId":"2","dataObject":{"content":"Jen purchased 10 shares of the stock on Janurary 1st, 2020."},"annotations":[{"workerId":"private.us-west-2.0123456789","annotationData":{"content":"{\"crowd-entity-annotation\":{\"entities\":[{\"endOffset\":3,\"label\":\"person\",\"startOffset\":0},{\"endOffset\":13,\"label\":\"verb\",\"startOffset\":4},{\"endOffset\":16,\"label\":\"number\",\"startOffset\":14},{\"endOffset\":58,\"label\":\"date\",\"startOffset\":40}]}}"}}]},{"datasetObjectId":"3","dataObject":{"content":"The narrative was interesting, however the character development was weak."},"annotations":[{"workerId":"private.us-west-2.0123456789","annotationData":{"content":"{\"crowd-entity-annotation\":{\"entities\":[{\"endOffset\":29,\"label\":\"adjective\",\"startOffset\":18},{\"endOffset\":73,\"label\":\"adjective\",\"startOffset\":69}]}}"}}]}]
  ```
+ You must use the directions in [Grant Post\-Annotation Lambda Permissions to Access Annotation](sms-custom-templates-step3-lambda-permissions.md#sms-custom-templates-step3-postlambda-perms) to give your post\-annotation Lambda function's execution role permission to assume the SageMaker execution role you use to create the labeling job\. The post\-annotation Lambda function uses the SageMaker execution role to access the annotation data file, `sample-annotations.json`, in S3\.



## Test the Pre\-annotation Lambda Function<a name="sms-custom-templates-step3-lambda-test-pre-annotation"></a>

Use the following procedure to test the pre\-annotation Lambda function created when you deployed the Ground Truth AWS Serverless Application Repository \(SAR\) recipe\. 

**Test the Ground Truth SAR recipe pre\-annotation Lambda function**

1. Open the [**Functions** page](https://console.aws.amazon.com/lambda/home#/functions) in the Lambda console\.

1. Select the pre\-annotation function that was deployed from the Ground Truth SAR recipe\. The name of this function is similar to `serverlessrepo-aws-sagema-GtRecipePreHumanTaskFunc-<id>`\.

1. In the **Code source** section, select the arrow next to **Test**\.

1. Select **Configure test event**\.

1. Keep the **Create new test event** option selected\.

1. Under **Event template**, select **SageMaker Ground Truth PreHumanTask**\. 

1. Give your test an **Event name**\.

1. Select **Create**\.

1. Select the arrow next to **Test** again and you should see that the test you created is selected, which is indicated with a dot by the event name\. If it is not selected, select it\. 

1. Select **Test** to run the test\. 

After you run the test, you can see the **Execution results**\. In the **Function logs**, you should see a response similar to the following:

```
START RequestId: cd117d38-8365-4e1a-bffb-0dcd631a878f Version: $LATEST
Received event: {
  "version": "2018-10-16",
  "labelingJobArn": "arn:aws:sagemaker:us-east-2:123456789012:labeling-job/example-job",
  "dataObject": {
    "source-ref": "s3://sagemakerexample/object_to_annotate.jpg"
  }
}
{'taskInput': {'taskObject': 's3://sagemakerexample/object_to_annotate.jpg'}, 'isHumanAnnotationRequired': 'true'}
END RequestId: cd117d38-8365-4e1a-bffb-0dcd631a878f
REPORT RequestId: cd117d38-8365-4e1a-bffb-0dcd631a878f	Duration: 0.42 ms	Billed Duration: 1 ms	Memory Size: 128 MB	Max Memory Used: 43 MB
```

In this response, we can see the Lambda function's output matches the required pre\-annotation response syntax:

```
{'taskInput': {'taskObject': 's3://sagemakerexample/object_to_annotate.jpg'}, 'isHumanAnnotationRequired': 'true'}
```

## Test the Post\-Annotation Lambda Function<a name="sms-custom-templates-step3-lambda-test-post-annotation"></a>

Use the following procedure to test the post\-annotation Lambda function created when you deployed the Ground Truth AWS Serverless Application Repository \(SAR\) recipe\. 

**Test the Ground Truth SAR recipe post\-annotation Lambda**

1. Open the [**Functions** page](https://console.aws.amazon.com/lambda/home#/functions) in the Lambda console\.

1. Select the post\-annotation function that was deployed from the Ground Truth SAR recipe\. The name of this function is similar to `serverlessrepo-aws-sagema-GtRecipeAnnotationConsol-<id>`\.

1. In the **Code source** section, select the arrow next to **Test**\.

1. Select **Configure test event**\.

1. Keep the **Create new test event** option selected\.

1. Under **Event template**, select **SageMaker Ground Truth AnnotationConsolidation**\.

1. Give your test an **Event name**\.

1. Modify the template code provided as follows:
   + Replace the Amazon Resource Name \(ARN\) in `roleArn` with the ARN of the SageMaker execution role you used to create the labeling job\.
   + Replace the S3 URI in `s3Uri` with the URI of the `sample-annotations.json` file you added to Amazon S3\.

   After you make these modifications, your test should look similar to the following:

   ```
   {
     "version": "2018-10-16",
     "labelingJobArn": "arn:aws:sagemaker:us-east-2:123456789012:labeling-job/example-job",
     "labelAttributeName": "example-attribute",
     "roleArn": "arn:aws:iam::222222222222:role/sm-execution-role",
     "payload": {
       "s3Uri": "s3://your-bucket/sample-annotations.json"
     }
   }
   ```

1. Select **Create**\.

1. Select the arrow next to **Test** again and you should see that the test you created is selected, which is indicated with a dot by the event name\. If it is not selected, select it\. 

1. Select the **Test** to run the test\. 

After you run the test, you should see a `-- Consolidated Output --` section in the **Function Logs**, which contains a list of all annotations included in `sample-annotations.json`\.