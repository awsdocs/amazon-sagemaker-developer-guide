# Step 3: Processing with AWS Lambda<a name="sms-custom-templates-step3"></a>

In this step, you set which AWS Lambda functions to trigger on each dataset object prior to sending it to workers and which function will be used to process the results once the task is submitted\. These functions are required\.

You will first need to visit the AWS Lambda console or use AWS Lambda's APIs to create your functions\. The AmazonSageMakerFullAccess policy is restricted to invoking AWS Lambda functions with one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-processing and post\-processing Lambdas\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the IAM role used for creating the labeling job\.

Select your lambdas from the **Lambda functions** section that comes after the code editor for your custom HTML in the Ground Truth console\.

If you need an example, there is an end\-to\-end demo, including Python code for the Lambdas, in the "[Demo Template: Annotation of Images with `crowd-bounding-box`](sms-custom-templates-step2-demo1.md)" document\.

## Pre\-labeling task Lambda<a name="sms-custom-templates-step3-prelambda"></a>

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

The `dataObject` will contain the `source` or `source-ref` property specified in the data object from your manifest\.

In return, Ground Truth will require a response formatted like this:

**Example of expected return data**  

```
{
    "taskInput": <json object>,
    "isHumanAnnotationRequired": <boolean> # Optional
}
```

That `<json object>` may be a bit deceiving\. It needs to contain *all* the data your custom form will need\. If you're doing a bounding box task where the instructions stay the same all the time, it may just be the HTTP\(S\) or S3 resource for your image file\. If it's a sentiment analysis task and different objects may have different choices, it would be the object reference as a string and the choices as an array of strings\.

**Implications of `isHumanAnnotationRequired`**  
 This value is optional because it will default to `true`\. The primary use case for explicitly setting it is when you want to exclude this data object from being labeled by human workers\. 

**The pre\-processing Lambda runs first**  
 Before any tasks are available to workers, your entire manifest will be processed into an intermediate form, using your Lambda\. This means you won't be able to change your Lambda part of the way through a labeling job and see that have an impact on the remaining tasks\. 

## Post\-labeling task Lambda<a name="sms-custom-templates-step3-postlambda"></a>

When the worker has completed the task, Ground Truth will send the results to your **Post\-labeling task Lambda**\. This Lambda is generally used for [Annotation Consolidation](sms-annotation-consolidation.md)\. The request object will come in like this:

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

### Post\-labeling task Lambda permissions<a name="sms-custom-templates-step3-postlambda-perms"></a>

The actual annotation data will be in a file designated by the `s3Uri` string in the `payload` object\. To process the annotations as they come in, even for a simple passthrough function, you need to assign the necessary permissions to your Lambda to read files from your S3 bucket\.

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