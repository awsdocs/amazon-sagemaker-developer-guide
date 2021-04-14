# Pre\-annotation and Post\-annotation Lambda Function Requirements<a name="sms-custom-templates-step3-lambda-requirements"></a>

Use this section to learn about the syntax of the requests sent to pre\-annotation and post\-annotation Lambda functions, and the response syntax that Ground Truth requires to run a custom labeling workflow\.

**Topics**
+ [Pre\-annotation Lambda](#sms-custom-templates-step3-prelambda)
+ [Post\-annotation Lambda](#sms-custom-templates-step3-postlambda)

## Pre\-annotation Lambda<a name="sms-custom-templates-step3-prelambda"></a>

Before a labeling task is sent to the worker, your pre\-annotation Lambda function is invoked\. 

Ground Truth sends your Lambda function a JSON\-formatted request to provide details about the labeling job and the data object\. The following table contains the pre\-annotation request schemas\. Each parameter is described below\.

------
#### [ Data object identified with "source\-ref" ]

```
{
    "version": "2018-10-16",
    "labelingJobArn": <labelingJobArn>
    "dataObject" : {
        "source-ref": <s3Uri>
    }
}
```

------
#### [ Data object identified with "source" ]

```
{
    "version": "2018-10-16",
    "labelingJobArn": <labelingJobArn>
    "dataObject" : {
        "source": <string>
    }
}
```

------
+ `version` \(string\): This is a version number used internally by Ground Truth\.
+ `labelingJobArn` \(string\): This is the Amazon Resource Name, or ARN, of your labeling job\. This ARN can be used to reference the labeling job when using Ground Truth API operations such as `DescribeLabelingJob`\.
+ The `dataObject` \(JSON object\): The key contains a single JSON line, either from your input manifest file or sent from Amazon SNS\. The JSON line objects in your manifest can be up to 100 kilobytes in size and contain a variety of data\. For a very basic image annotation job, the `dataObject` JSON may just contain a `source-ref` key, identifying the image to be annotated\. If the data object \(for example, a line of text\) is included directly in the input manifest file, the data object is identified with `source`\. If you create a verification or adjustment job, this line may contain label data and metadata from the previous labeling job\.

The following table includes code block examples of a pre\-annotation request\. Each parameter in these example requests is explained below the tabbed table\.

------
#### [ Data object identified with "source\-ref" ]

```
{
    "version": "2018-10-16",
    "labelingJobArn": "arn:aws:sagemaker:<aws_region>:<aws_account_number>:labeling-job/<labeling_job_name>"
    "dataObject" : {
        "source-ref": "s3://<input-data-bucket>/<data-object-file-name>"
    }
}
```

------
#### [ Data object identified with "source" ]

```
{
    "version": "2018-10-16",
    "labelingJobArn": "arn:aws:sagemaker:<aws_region>:<aws_account_number>:labeling-job/<labeling_job_name>"
    "dataObject" : {
        "source": "Sue purchased 10 shares of the stock on April 10th, 2020"
    }
}
```

------

In return, Ground Truth requires a response formatted like the following:

**Example of expected return data**  

```
{
    "taskInput": <json object>,
    "isHumanAnnotationRequired": <boolean> # Optional
}
```

In the previous example, the `<json object>` needs to contain *all* the data your custom worker task template needs\. If you're doing a bounding box task where the instructions stay the same all the time, it may just be the HTTP\(S\) or Amazon S3 resource for your image file\. If it's a sentiment analysis task and different objects may have different choices, it is the object reference as a string and the choices as an array of strings\.

**Implications of `isHumanAnnotationRequired`**  
This value is optional because it defaults to `true`\. The primary use case for explicitly setting it is when you want to exclude this data object from being labeled by human workers\. 

If you have a mix of objects in your manifest, with some requiring human annotation and some not needing it, you can include a `isHumanAnnotationRequired` value in each data object\. You can add logic to your pre\-annotation Lambda to dynamically determine if an object requires annotation, and set this boolean value accordingly\.

### Examples of Pre\-annotation Lambda Functions<a name="sms-custom-templates-step3-prelambda-example"></a>

The following, basic pre\-annotation Lambda function accesses the JSON object in `dataObject` from the initial request, and returns it in the `taskInput` parameter\. 

```
import json

def lambda_handler(event, context):
    return {
        "taskInput":  event['dataObject']
    }
```

Assuming the input manifest file uses `"source-ref"` to identify data objects, the worker task template used in the same labeling job as this pre\-annotation Lambda must include a Liquid element like the following to ingest `dataObject`:

```
{{ task.input.source-ref | grant_read_access }}
```

If the input manifest file used `source` to identify the data object, the work task template can ingest `dataObject` with the following:

```
{{ task.input.source }}
```

The following pre\-annotation Lambda example includes logic to identify the key used in `dataObject`, and to point to that data object using `taskObject` in the Lambda's return statement\.

```
import json

def lambda_handler(event, context):

    # Event received
    print("Received event: " + json.dumps(event, indent=2))

    # Get source if specified
    source = event['dataObject']['source'] if "source" in event['dataObject'] else None

    # Get source-ref if specified
    source_ref = event['dataObject']['source-ref'] if "source-ref" in event['dataObject'] else None

    # if source field present, take that otherwise take source-ref
    task_object = source if source is not None else source_ref

    # Build response object
    output = {
        "taskInput": {
            "taskObject": task_object
        },
        "humanAnnotationRequired": "true"
    }

    print(output)
    # If neither source nor source-ref specified, mark the annotation failed
    if task_object is None:
        print(" Failed to pre-process {} !".format(event["labelingJobArn"]))
        output["humanAnnotationRequired"] = "false"

    return output
```

## Post\-annotation Lambda<a name="sms-custom-templates-step3-postlambda"></a>

When all workers have annotated the data object or when [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanLoopConfig.html#SageMaker-Type-HumanLoopConfig-TaskAvailabilityLifetimeInSeconds](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanLoopConfig.html#SageMaker-Type-HumanLoopConfig-TaskAvailabilityLifetimeInSeconds) has been reached, whichever comes first, Ground Truth sends those annotations to your post\-annotation Lambda\. This Lambda is generally used for [Consolidate Annotations ](sms-annotation-consolidation.md)\.

**Tip**  
To see an example of a post\-consolidation Lambda function, see [annotation\_consolidation\_lambda\.py](https://github.com/aws-samples/aws-sagemaker-ground-truth-recipe/blob/master/aws_sagemaker_ground_truth_sample_lambda/annotation_consolidation_lambda.py) in the [aws\-sagemaker\-ground\-truth\-recipe](https://github.com/aws-samples/aws-sagemaker-ground-truth-recipe) GitHub repository\.

The following code block contains the post\-annotation request schema\. Each parameter is described in the following bulleted list\.

```
{
    "version": "2018-10-16",
    "labelingJobArn": <string>,
    "labelCategories": [<string>],
    "labelAttributeName": <string>,
    "roleArn" : <string>,
    "payload": {
        "s3Uri": <string>
    }
 }
```
+ `version` \(string\): A version number used internally by Ground Truth\.
+ `labelingJobArn` \(string\): The Amazon Resource Name, or ARN, of your labeling job\. This ARN can be used to reference the labeling job when using Ground Truth API operations such as `DescribeLabelingJob`\.
+ `labelCategories` \(list of strings\): Includes the label categories and other attributes you either specified in the console, or that you include in the label category configuration file\.
+ `labelAttributeName` \(string\): Either the name of your labeling job, or the label attribute name you specify when you create the labeling job\.
+ `roleArn` \(string\): The Amazon Resource Name \(ARN\) of the IAM execution role you specify when you create the labeling job\. 
+ `payload` \(JSON object\): A JSON that includes an `s3Uri` key, which identifies the location of the annotation data for that data object in Amazon S3\. The second code block below shows an example of this annotation file\.

The following code block contains an example of a post\-annotation request\. Each parameter in this example request is explained below the code block\.

**Example of an post\-annotation Lambda request**  

```
{
    "version": "2018-10-16",
    "labelingJobArn": "arn:aws:sagemaker:us-west-2:111122223333:labeling-job/labeling-job-name",
    "labelCategories": ["Ex Category1","Ex Category2", "Ex Category3"],
    "labelAttributeName": "labeling-job-attribute-name",
    "roleArn" : "arn:aws:iam::111122223333:role/role-name",
    "payload": {
        "s3Uri": "s3://DOC-EXAMPLE-BUCKET/annotations.json"
    }
 }
```

**Note**  
If no worker works on the data object and `TaskAvailabilityLifetimeInSeconds` has been reached, the data object is marked as failed and not included as part of post\-annotation Lambda invocation\.

The following code block contains the payload schema\. This is the file that is indicated by the `s3Uri` parameter in the post\-annotation Lambda request `payload` JSON object\. For example, if the previous code block is the post\-annotation Lambda request, the following annotation file is located at `s3://DOC-EXAMPLE-BUCKET/annotations.json`\.

Each parameter is described in the following bulleted list\.

**Example of an annotation file**  

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
+ `datasetObjectId` \(string\): Identifies a unique ID that Ground Truth assigns to each data object you send to the labeling job\.
+ `dataObject` \(JSON object\): The data object that was labeled\. If the data object is included in the input manifest file and identified using the `source` key \(for example, a string\), `dataObject` includes a `content` key, which identifies the data object\. Otherwise, the location of the data object \(for example, a link or S3 URI\) is identified with `s3Uri`\.
+ `annotations` \(list of JSON objects\): This list contains a single JSON object for each annotation submitted by workers for that `dataObject`\. A single JSON object contains a unique `workerId` that can be used to identify the worker that submitted that annotation\. The `annotationData` key contains one of the following:
  + `content` \(string\): Contains the annotation data\. 
  + `s3Uri` \(string\): Contains an S3 URI that identifies the location of the annotation data\.

The following table contains examples of the content that you may find in payload for different types of annotation\.

------
#### [ Named Entity Recognition Payload ]

```
[
    {
      "datasetObjectId": "1",
      "dataObject": {
        "content": "Sift 3 cups of flour into the bowl."
      },
      "annotations": [
        {
          "workerId": "private.us-west-2.ef7294f850a3d9d1",
          "annotationData": {
            "content": "{\"crowd-entity-annotation\":{\"entities\":[{\"endOffset\":4,\"label\":\"verb\",\"startOffset\":0},{\"endOffset\":6,\"label\":\"number\",\"startOffset\":5},{\"endOffset\":20,\"label\":\"object\",\"startOffset\":15},{\"endOffset\":34,\"label\":\"object\",\"startOffset\":30}]}}"
          }
        }
      ]
    }
]
```

------
#### [ Semantic Segmentation Payload ]

```
[
    {
      "datasetObjectId": "2",
      "dataObject": {
        "s3Uri": "s3://DOC-EXAMPLE-BUCKET/gt-input-data/images/bird3.jpg"
      },
      "annotations": [
        {
          "workerId": "private.us-west-2.ab1234c5678a919d0",
          "annotationData": {
            "content": "{\"crowd-semantic-segmentation\":{\"inputImageProperties\":{\"height\":2000,\"width\":3020},\"labelMappings\":{\"Bird\":{\"color\":\"#2ca02c\"}},\"labeledImage\":{\"pngImageData\":\"iVBOR...\"}}}"
          }
        }
      ]
    }
  ]
```

------
#### [ Bounding Box Payload ]

```
[
    {
      "datasetObjectId": "0",
      "dataObject": {
        "s3Uri": "s3://DOC-EXAMPLE-BUCKET/gt-input-data/images/bird1.jpg"
      },
      "annotations": [
        {
          "workerId": "private.us-west-2.ab1234c5678a919d0",
          "annotationData": {
            "content": "{\"boundingBox\":{\"boundingBoxes\":[{\"height\":2052,\"label\":\"Bird\",\"left\":583,\"top\":302,\"width\":1375}],\"inputImageProperties\":{\"height\":2497,\"width\":3745}}}"
          }
        }
      ]
    }
 ]
```

------

Your post\-annotation Lambda function may contain logic similar to the following to loop through and access all annotations contained in the request\. For a full example, see [annotation\_consolidation\_lambda\.py](https://github.com/aws-samples/aws-sagemaker-ground-truth-recipe/blob/master/aws_sagemaker_ground_truth_sample_lambda/annotation_consolidation_lambda.py) in the [aws\-sagemaker\-ground\-truth\-recipe](https://github.com/aws-samples/aws-sagemaker-ground-truth-recipe) GitHub repository\. In this GitHub example, you must add your own annotation consolidation logic\. 

```
for i in range(len(annotations)):
    worker_id = annotations[i]["workerId"]
    annotation_content = annotations[i]['annotationData'].get('content')
    annotation_s3_uri = annotations[i]['annotationData'].get('s3uri')
    annotation = annotation_content if annotation_s3_uri is None else s3_client.get_object_from_s3(
        annotation_s3_uri)
    annotation_from_single_worker = json.loads(annotation)

    print("{} Received Annotations from worker [{}] is [{}]"
            .format(log_prefix, worker_id, annotation_from_single_worker))
```

**Tip**  
When you run consolidation algorithms on the data, you can use an AWS database service to store results, or you can pass the processed results back to Ground Truth\. The data you return to Ground Truth is stored in consolidated annotation manifests in the S3 bucket specified for output during the configuration of the labeling job\.

In return, Ground Truth requires a response formatted like the following:

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
At this point, all the data you're sending to your S3 bucket, other than the `datasetObjectId`, is in the `content` object\.

When you return annotations in `content`, this results in an entry in your job's output manifest like the following:

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

Because of the potentially complex nature of a custom template and the data it collects, Ground Truth does not offer further processing of the data\.