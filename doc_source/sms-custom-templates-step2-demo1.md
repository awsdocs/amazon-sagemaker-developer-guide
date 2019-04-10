# Demo Template: Annotation of Images with `crowd-bounding-box`<a name="sms-custom-templates-step2-demo1"></a>

After you have selected to use a custom template, you'll reach the **Custom labeling task panel**\. There you can choose from multiple base templates that represent some of the more common tasks and will provide a sample to work from in building your customized labeling task's template\.

In this demonstration, you'll work with the **BoundingBox** template and the AWS Lambda functions needed for processing your data before and after the task\.

**Example : Base Bounding Box Custom Template**  
This is the Bounding Box template that is provided\.  

```
<script src="https://assets.crowd.aws/crowd-html-elements.jss"></script>
<crowd-form>
  <crowd-bounding-box
    name="boundingBox"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="{{ task.input.header }}"
    labels="{{ task.input.labels | to_json | escape }}"
  >
    <full-instructions header="Bounding box instructions">
      Sample Full Instructions with examples
    </full-instructions>
    <short-instructions>
      Sample Short Instructions
    </short-instructions>
  </crowd-bounding-box>
</crowd-form>
```

The custom templates use the [Liquid template language](https://shopify.github.io/liquid/), and each of the items between double curly braces is a variable\. The pre\-processing AWS Lambda function should provide an object named `taskInput` and that object's properties can be accessed as `{{ task.input.<property name> }}` in your template\.

In the base sample, there are three variables: `taskObject`, `header`, and `labels`\.

Each of these would be represented in different parts of the bounding box\.
+ `taskObject` is an HTTP\(S\) URL or S3 URI for the photo to be annotated\. The added `| grant_read_access` is a filter that will convert an S3 URI to an HTTPS URL with short\-lived access to that resource\. If you're using an HTTP\(S\) URL, it's not needed\.
+ `header` is the text above the photo to be labeled, something like "Draw a box around the bird in the photo\."
+ `labels` is an array, represented as `['item1', 'item2', ...]`\. These are labels that can be assigned by the worker to the different boxes they draw\. You can have one or many\.

Each of the variable names come from the JSON object in the response from your pre\-processing Lambda, The names above are merely suggested, Use whatever variable names make sense to you and will promote code readability among your team\.

**Only use variables when necessary**  
If a field will not change, you can remove that variable from the template and replace it with that text, otherwise you have to repeat that text as a value in each object in your manifest\.

**Example : Final Customized Bounding Box Template**  
To keep things simple, this template will have one variable, one label, and very basic instructions\.  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-bounding-box
    name="annotatedResult"
    labels="['bird']"
    src="{{ task.input.sourceRef | grant_read_access }}"
    header="Draw a box around the bird"
  >
    <full-instructions header="Bounding Box Instructions" >
      <p>Draw a bounding box around the bird in each image. If 
      there is more than one bird per image, draw a bounding 
      box around the largest one.</p>
      <p>Bounding boxes should be tight around the bird with 
      no more than a couple of pixels of buffer around the 
      edges.</p>
      <p>If there are no birds in the image, check the <strong>
      Nothing to label</strong> box.
    </full-instructions>
    <short-instructions>
      <p>Draw a bounding box around the bird in each image. If 
      there is more than one bird per image, draw a bounding 
      box around the largest one.</p>
    </short-instructions>
  </crowd-bounding-box>
</crowd-form>
```
One difference to call out is that the variable is `task.input.sourceRef` instead of `task.input.taskObject`\. This is to show that variable naming is flexible and should be semantically meaningful to your developers, but it does have to correspond to a field passed from your pre\-processing Lambda\.

**Example : Your manifest file**  
Your manifest file should key to the variables you're using in your template\. You can do some transformation of your manifest data in your pre\-processing Lambda, but if you don't need to, you maintain a lower risk of errors and your Lambda will run faster\. Here's a sample manifest file for the template\.  

```
{"source-ref": "<S3 image URI>"}
{"source-ref": "<S3 image URI>"}
{"source-ref": "<S3 image URI>"}
{"source-ref": "<S3 image URI>"}
```

**Example : Your Pre\-labeling task Lambda function**  
As part of the job set\-up, you'll need to provide the ARN of an AWS Lambda that can be called to process your manifest entries and pass them to the template engine\. The AmazonSageMakerFullAccess policy is restricted to invoking AWS Lambda functions with one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-processing and post\-processing Lambdas\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the IAM role used for creating the labeling job\.   
This applies to both your pre\-processing and post\-processing Lambdas\.  
When you're using the console, if you have Lambdas that are owned by your account, a drop\-down list of functions meeting the naming requirements will be provided to choose one\.  
In this very basic sample, where you have only one variable, it's primarily a pass\-through function\. Here's a sample pre\-processing Lambda in Python 3\.7\.  

```
import json

def lambda_handler(event, context):
    return {
        "taskInput": {
            "sourceRef" : event['dataObject']['source-ref']
        }
    }
```
The API contract for the request and response is provided on the page about [pre\-processing and post\-processing Lambdas](sms-custom-templates-step3.md)\. The JSON object from your manifest will be provided as a child of the `event` object\. In this demonstration, it is a simple passthrough of one variable, but you might want to have a custom header per object, custom examples, etc\. The properties inside the `taskInput` object will be available as variables to your template\.

**Example : Your Post\-labeling task Lambda function**  
As part of the job set\-up, you'll need to provide the ARN of an AWS Lambda that can be called to process the form data when a worker completes a task\. This can be as simple or complex as you want\. If you want to do answer consolidation and scoring as it comes in, you can apply the scoring and/or consolidation algorithms of your choice\. If you want to store the raw data for offline processing, that is an option\.  
The annotation data will be in a file designated by the `s3Uri` string in the `payload` object\. To process the annotations as they come in, even for a simple passthrough function, you need to assign `S3ReadOnly` access to your Lambda so it can read the annotation files\.  
In the Console page for creating your Lambda, scroll to the **Execution role** panel\. Select **Create a new role from one or more templates**\. Give the role a name\. From the **Policy templates** drop\-down, choose **Amazon S3 object read\-only permissions**\. Save the Lambda and the role will be saved and selected\.  
The following sample is in Python 2\.7\.  

```
import json
import boto3
from urlparse import urlparse

def lambda_handler(event, context):
    consolidated_labels = []

    parsed_url = urlparse(event['payload']['s3Uri']);
    s3 = boto3.client('s3')
    textFile = s3.get_object(Bucket = parsed_url.netloc, Key = parsed_url.path[1:])
    filecont = textFile['Body'].read()
    annotations = json.loads(filecont);
    
    for dataset in annotations:
        for annotation in dataset['annotations']:
            new_annotation = json.loads(annotation['annotationData']['content'])
            label = {
                'datasetObjectId': dataset['datasetObjectId'],
                'consolidatedAnnotation' : {
                'content': {
                    event['labelAttributeName']: {
                        'workerId': annotation['workerId'],
                        'boxesInfo': new_annotation,
                        'imageSource': dataset['dataObject']
                        }
                    }
                }
            }
            consolidated_labels.append(label)
    
    return consolidated_labels
```

The post\-processing Lambda will often receive batches of task results in the event object\. That batch will be the `payload` object the Lambda should iterate through\. What you'll send back will be an object meeting the [API contract](sms-custom-templates-step3.md)\.

You'll find the output of the job in a folder named after your labeling job in the target S3 bucket you specified\. It will be in a subfolder named `manifests`\.

For a bounding box task, the output you'll find in the output manifest will look a bit like the demo below\. The example has been cleaned up for printing\. The actual output will be a single line per record\.

**Example : JSON in your output manifest**  

```
{
  "source":"<URL>",
  "<Labeling job name>":
    {
       "workerId":"<URL>",
       "imageSource":"<image URL>",
        "boxesInfo":"{\"annotatedResult\":{\"boundingBoxes\":[{\"height\":878, \"label\":\"bird\", \"left\":208, \"top\":6, \"width\":809}], \"inputImageProperties\":{\"height\":924, \"width\":1280}}}"},
  "<Labeling job name>-metadata":
    {
      "type":"groundTruth/custom",
      "job_name":"<Labeling job name>",
      "human-annotated":"yes"
    }
}
```
This should help you create your own custom template\.