# Demo Template: Annotation of Images with `crowd-bounding-box`<a name="sms-custom-templates-step2-demo1"></a>

When you chose to use a custom template as your task type in the Amazon SageMaker Ground Truth console, you reach the **Custom labeling task panel**\. There you can choose from multiple base templates\. The templates represent some of the most common tasks and provide a sample to work from as you create your customized labeling task's template\. If you are not using the console, or as an additional recourse, see [Amazon SageMaker Ground Truth Sample Task UIs ](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis) for a repository of demo templates for a variety of labeling job task types\.

This demonstration works with the **BoundingBox** template\. The demonstration also works with the AWS Lambda functions needed for processing your data before and after the task\. In the Github repository above, to find templates that work with AWS Lambdafunctions, look for `{{ task.input.<property name> }}` in the template\.

**Topics**
+ [Starter Bounding Box custom template](#sms-custom-templates-step2-demo1-base-template)
+ [Your own Bounding Box custom template](#sms-custom-templates-step2-demo1-your-own-template)
+ [Your manifest file](#sms-custom-templates-step2-demo1-manifest)
+ [Your pre\-annotation Lambda function](#sms-custom-templates-step2-demo1-pre-annotation)
+ [Your post\-annotation Lambda function](#sms-custom-templates-step2-demo1-post-annotation)
+ [The output of your labeling job](#sms-custom-templates-step2-demo1-job-output)

## Starter Bounding Box custom template<a name="sms-custom-templates-step2-demo1-base-template"></a>

This is the starter bounding box template that is provided\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-bounding-box
    name="boundingBox"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="{{ task.input.header }}"
    labels="{{ task.input.labels | to_json | escape }}"
  >

    <!-- The <full-instructions> tag is where you will define the full instructions of your task. -->
    <full-instructions header="Bounding Box Instructions" >
      <p>Use the bounding box tool to draw boxes around the requested target of interest:</p>
      <ol>
        <li>Draw a rectangle using your mouse over each instance of the target.</li>
        <li>Make sure the box does not cut into the target, leave a 2 - 3 pixel margin</li>
        <li>
          When targets are overlapping, draw a box around each object,
          include all contiguous parts of the target in the box.
          Do not include parts that are completely overlapped by another object.
        </li>
        <li>
          Do not include parts of the target that cannot be seen,
          even though you think you can interpolate the whole shape of the target.
        </li>
        <li>Avoid shadows, they're not considered as a part of the target.</li>
        <li>If the target goes off the screen, label up to the edge of the image.</li>
      </ol>
    </full-instructions>

    <!-- The <short-instructions> tag allows you to specify instructions that are displayed in the left hand side of the task interface.
    It is a best practice to provide good and bad examples in this section for quick reference. -->
    <short-instructions>
      Use the bounding box tool to draw boxes around the requested target of interest.
    </short-instructions>
  </crowd-bounding-box>
</crowd-form>
```

The custom templates use the [Liquid template language](https://shopify.github.io/liquid/), and each of the items between double curly braces is a variable\. The pre\-annotation AWS Lambda function should provide an object named `taskInput` and that object's properties can be accessed as `{{ task.input.<property name> }}` in your template\.

## Your own Bounding Box custom template<a name="sms-custom-templates-step2-demo1-your-own-template"></a>

As an example, assume you have a large collection of animal photos in which you know the kind of animal in an image from a prior image\-classification job\. Now you want to have a bounding box drawn around it\.

In the starter sample, there are three variables: `taskObject`, `header`, and `labels`\.

Each of these would be represented in different parts of the bounding box\.
+ `taskObject` is an HTTP\(S\) URL or S3 URI for the photo to be annotated\. The added `| grant_read_access` is a filter that will convert an S3 URI to an HTTPS URL with short\-lived access to that resource\. If you're using an HTTP\(S\) URL, it's not needed\.
+ `header` is the text above the photo to be labeled, something like "Draw a box around the bird in the photo\."
+ `labels` is an array, represented as `['item1', 'item2', ...]`\. These are labels that can be assigned by the worker to the different boxes they draw\. You can have one or many\.

Each of the variable names come from the JSON object in the response from your pre\-annotation Lambda, The names above are merely suggested, Use whatever variable names make sense to you and will promote code readability among your team\.

**Only use variables when necessary**  
If a field will not change, you can remove that variable from the template and replace it with that text, otherwise you have to repeat that text as a value in each object in your manifest or code it into your pre\-annotation Lambda function\.

**Example : Final Customized Bounding Box Template**  
To keep things simple, this template will have one variable, one label, and very basic instructions\. Assuming your manifest has an "animal" property in each data object, that value can be re\-used in two parts of the template\.  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-bounding-box
    name="boundingBox"
    labels="[ '{{ task.input.animal }}' ]"
    src="{{ task.input.source-ref | grant_read_access }}"
    header="Draw a box around the {{ task.input.animal }}."
  >
    <full-instructions header="Bounding Box Instructions" >
      <p>Draw a bounding box around the {{ task.input.animal }} in the image. If 
      there is more than one {{ task.input.animal }} per image, draw a bounding 
      box around the largest one.</p>
      <p>The box should be tight around the {{ task.input.animal }} with 
      no more than a couple of pixels of buffer around the 
      edges.</p>
      <p>If the image does not contain a {{ task.input.animal }}, check the <strong>
      Nothing to label</strong> box.
    </full-instructions>
    <short-instructions>
      <p>Draw a bounding box around the {{ task.input.animal }} in each image. If 
      there is more than one {{ task.input.animal }} per image, draw a bounding 
      box around the largest one.</p>
    </short-instructions>
  </crowd-bounding-box>
</crowd-form>
```
Note the re\-use of `{{ task.input.animal }}` throughout the template\. If your manifest had all of the animal names beginning with a capital letter, you could use `{{ task.input.animal | downcase }}`, incorporating one of Liquid's built\-in filters in sentences where it needed to be presented lowercase\.

## Your manifest file<a name="sms-custom-templates-step2-demo1-manifest"></a>

Your manifest file should provide the variable values you're using in your template\. You can do some transformation of your manifest data in your pre\-annotation Lambda, but if you don't need to, you maintain a lower risk of errors and your Lambda will run faster\. Here's a sample manifest file for the template\.

```
{"source-ref": "<S3 image URI>", "animal": "horse"}
{"source-ref": "<S3 image URI>", "animal" : "bird"}
{"source-ref": "<S3 image URI>", "animal" : "dog"}
{"source-ref": "<S3 image URI>", "animal" : "cat"}
```

## Your pre\-annotation Lambda function<a name="sms-custom-templates-step2-demo1-pre-annotation"></a>

As part of the job set\-up, provide the ARN of an AWS Lambda function that can be called to process your manifest entries and pass them to the template engine\.

**Naming your Lambda function**  
The best practice in naming your function is to use one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-annotation and post\-annotation functions\.

When you're using the console, if you have AWS Lambda functions that are owned by your account, a drop\-down list of functions meeting the naming requirements will be provided to choose one\.

In this very basic example, you're just passing through the information from the manifest without doing any additional processing on it\. This sample pre\-annotation function is written for Python 3\.7\.

```
import json

def lambda_handler(event, context):
    return {
        "taskInput": event['dataObject']
    }
```

The JSON object from your manifest will be provided as a child of the `event` object\. The properties inside the `taskInput` object will be available as variables to your template, so simply setting the value of `taskInput` to `event['dataObject']` will pass all the values from your manifest object to your template without having to copy them individually\. If you wish to send more values to the template, you can add them to the `taskInput` object\.

## Your post\-annotation Lambda function<a name="sms-custom-templates-step2-demo1-post-annotation"></a>

As part of the job set\-up, provide the ARN of an AWS Lambda function that can be called to process the form data when a worker completes a task\. This can be as simple or complex as you want\. If you want to do answer consolidation and scoring as it comes in, you can apply the scoring and/or consolidation algorithms of your choice\. If you want to store the raw data for offline processing, that is an option\.

**Provide permissions to your post\-annotation Lambda**  
The annotation data will be in a file designated by the `s3Uri` string in the `payload` object\. To process the annotations as they come in, even for a simple pass through function, you need to assign `S3ReadOnly` access to your Lambda so it can read the annotation files\.  
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

The post\-annotation Lambda will often receive batches of task results in the event object\. That batch will be the `payload` object the Lambda should iterate through\. What you send back will be an object meeting the [API contract](sms-custom-templates-step3.md)\.

## The output of your labeling job<a name="sms-custom-templates-step2-demo1-job-output"></a>

You'll find the output of the job in a folder named after your labeling job in the target S3 bucket you specified\. It will be in a subfolder named `manifests`\.

For a bounding box task, the output you find in the output manifest will look a bit like the demo below\. The example has been cleaned up for printing\. The actual output will be a single line per record\.

**Example : JSON in your output manifest**  

```
{
  "source-ref":"<URL>",
  "<label attribute name>":
    {
       "workerId":"<URL>",
       "imageSource":"<image URL>",
        "boxesInfo":"{\"boundingBox\":{\"boundingBoxes\":[{\"height\":878, \"label\":\"bird\", \"left\":208, \"top\":6, \"width\":809}], \"inputImageProperties\":{\"height\":924, \"width\":1280}}}"},
  "<label attribute name>-metadata":
    {
      "type":"groundTruth/custom",
      "job_name":"<Labeling job name>",
      "human-annotated":"yes"
    },
  "animal" : "bird"
}
```
Note how the additional `animal` attribute from your original manifest is passed to the output manifest on the same level as the `source-ref` and labeling data\. Any properties from your input manifest, whether they were used in your template or not, will be passed to the output manifest\.  
This should help you create your own custom template\.