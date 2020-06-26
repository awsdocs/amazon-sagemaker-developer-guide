# Step 3: Processing with AWS Lambda<a name="sms-custom-templates-step3"></a>

In this step, you set which AWS Lambda functions to trigger on each dataset object prior to sending it to workers and which function will be used to process the results once the task is submitted\. These functions are required\.

You will first need to visit the AWS Lambda console or use AWS Lambda's APIs to create your functions\. The AmazonSageMakerFullAccess policy is restricted to invoking AWS Lambda functions with one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-annotation and post\-annotation Lambdas\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the IAM role used for creating the labeling job\.

Select your lambdas from the **Lambda functions** section that comes after the code editor for your custom HTML in the Ground Truth console\.

If you need an example, there is an end\-to\-end demo, including Python code for the Lambdas, in the "[Demo Template: Annotation of Images with `crowd-bounding-box`](sms-custom-templates-step2-demo1.md)" document\.

## Pre\-annotation Lambda<a name="sms-custom-templates-step3-prelambda"></a>

Before a labeling task is sent to the worker, your AWS Lambda function will be sent a JSON formatted request to provide details\.

**Example of a Pre\-annotation request**  

```
{
    "version": "2018-10-16",
    "labelingJobArn": <labelingJobArn>
    "dataObject" : {
        "source-ref": "s3://mybucket/myimage.png"
    }
}
```

The `dataObject` will contain the JSON formatted properties from your manifest's data object\. For a very basic image annotation job, it might just be a `source-ref` property specifying the image to be annotated\. The JSON line objects in your manifest can be up to 100 kilobytes in size and contain a variety of data\.

In return, Ground Truth will require a response formatted like this:

**Example of expected return data**  

```
{
    "taskInput": <json object>,
    "isHumanAnnotationRequired": <boolean> # Optional
}
```

In the previous example, the `<json object>` needs to contain *all* the data your custom form will need\. If you're doing a bounding box task where the instructions stay the same all the time, it may just be the HTTP\(S\) or S3 resource for your image file\. If it's a sentiment analysis task and different objects may have different choices, it would be the object reference as a string and the choices as an array of strings\.

**Implications of `isHumanAnnotationRequired`**  
 This value is optional because it will default to `true`\. The primary use case for explicitly setting it is when you want to exclude this data object from being labeled by human workers\. 

If you have a mix of objects in your manifest, with some requiring human annotation and some not needing it, you can include a `isHumanAnnotationRequired` value in each data object\. You can then use code in your pre\-annotation Lambda to read the value from the data object and set the value in your Lambda output\.

**The pre\-annotation Lambda runs first**  
 Before any tasks are available to workers, your entire manifest will be processed into an intermediate form, using your Lambda\. This means you won't be able to change your Lambda part of the way through a labeling job and see that have an impact on the remaining tasks\. 

## Post\-annotation Lambda<a name="sms-custom-templates-step3-postlambda"></a>

When all workers have annotated the data object or when [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanLoopConfig.html#SageMaker-Type-HumanLoopConfig-TaskAvailabilityLifetimeInSeconds](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanLoopConfig.html#SageMaker-Type-HumanLoopConfig-TaskAvailabilityLifetimeInSeconds) has been reached, whichever comes first, Ground Truth will send those annotations to your Post\-annotation Lambda\. This Lambda is generally used for [Consolidate Annotations ](sms-annotation-consolidation.md)\. The request object will come in like this:

**Example of a post\-labeling task request**  

```
{
    "version": "2018-10-16",
    "labelingJobArn": <labelingJobArn>,
    "labelCategories": [<string>],
    "labelAttributeName": <string>,
    "roleArn" : "string",
    "payload": {
        "s3Uri": <string>
    }
 }
```
 If no worker work on the data object and `TaskAvailabilityLifetimeInSeconds` has been reached, data object will be marked as failed and not included as part of post annotation lambda invocation\.

### Post\-labeling task Lambda permissions<a name="sms-custom-templates-step3-postlambda-perms"></a>

The actual annotation data will be in a file designated by the `s3Uri` string in the `payload` object\. To process the annotations as they come in, even for a simple pass through function, you need to assign the necessary permissions to your Lambda to read files from your S3 bucket\.

In the Console page for creating your Lambda, scroll to the **Execution role** panel\. Select **Create a new role from one or more templates**\. Give the role a name\. From the **Policy templates** drop\-down, choose **Amazon S3 object read\-only permissions**\. Save the Lambda and the role will be saved and selected\.

**Example of an annotation data file**  

```
[
    {
        "datasetObjectId": <string>,
        "dataObject": {
            "s3Uri": <string>,
            "content": <string>
        },
        "annotations": [{
            "workerId": <string>,
            "annotationData": {
                "content": <string>,
                "s3Uri": <string>
            }
       }]
    }
]
```
Essentially, all the fields from your form will be in the `content` object\. At this point you can start running data consolidation algorithms on the data, using an AWS database service to store results\. Or you can pass some processed/optimized results back to Ground Truth for storage in your consolidated annotation manifests in the S3 bucket you specify for output during the configuration of the labeling job\.

In return, Ground Truth will require a response formatted like this:

**Example of expected return data**  

```
[
   {        
        "datasetObjectId": <string>,
        "consolidatedAnnotation": {
            "content": {
                "<labelattributename>": {
                    # ... label content
                }
            }
        }
    },
   {        
        "datasetObjectId": <string>,
        "consolidatedAnnotation": {
            "content": {
                "<labelattributename>": {
                    # ... label content
                }
            }
        }
    }
    .
    .
    .
]
```
At this point, all the data you're sending to your S3 bucket, other than the `datasetObjectId` will be in the `content` object\.

That will result in an entry in your job's consolidation manifest like this:

**Example of label format in output manifest**  

```
{  "source-ref"/"source" : "<s3uri or content>", 
   "<labelAttributeName>": {
        # ... label content from you
    },   
   "<labelAttributeName>-metadata": { # This will be added by Ground Truth
        "job_name": <labelingJobName>,
        "type": "groundTruth/custom",
        "human-annotated": "yes", 
        "creation_date": <date> # Timestamp of when received from Post-labeling Lambda
    }
}
```

Because of the potentially complex nature of a custom template and the data it collects, Ground Truth does not offer further processing of the data or insights into it\.

## Next<a name="templates-step3-next"></a>

[Custom Workflows via the API](sms-custom-templates-step4.md)