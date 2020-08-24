# Demo Template: Labeling Intents with `crowd-classifier`<a name="sms-custom-templates-step2-demo2"></a>

If you choose a custom template, you'll reach the **Custom labeling task panel**\. There you can select from multiple starter templates that represent some of the more common tasks\. The templates provide a starting point to work from in building your customized labeling task's template\.

In this demonstration, you work with the **Intent Detection** template, which uses the `` element, and the AWS Lambda functions needed for processing your data before and after the task\.

**Topics**
+ [Starter Intent Detection custom template](#sms-custom-templates-step2-demo2-base-template)
+ [Your Intent Detection custom template](#sms-custom-templates-step2-demo2-your-template)
+ [Your pre\-annotation Lambda function](#sms-custom-templates-step2-demo2-pre-lambda)
+ [Your post\-annotation Lambda function](#sms-custom-templates-step2-demo2-post-lambda)
+ [Your labeling job output](#sms-custom-templates-step2-demo2-job-output)

## Starter Intent Detection custom template<a name="sms-custom-templates-step2-demo2-base-template"></a>

This is the intent detection template that is provided as a starting point\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-classifier
    name="intent"
    categories="{{ task.input.labels | to_json | escape }}"
    header="Pick the most relevant intention expressed by the below text"
  >
    <classification-target>
      {{ task.input.utterance }}
    </classification-target>
    
    <full-instructions header="Intent Detection Instructions">
        <p>Select the most relevant intention expressed by the text.</p>
        <div>
           <p><strong>Example: </strong>I would like to return a pair of shoes</p>
           <p><strong>Intent: </strong>Return</p>
        </div>
    </full-instructions>

    <short-instructions>
      Pick the most relevant intention expressed by the text
    </short-instructions>
  </crowd-classifier>
</crowd-form>
```

The custom templates use the [Liquid template language](https://shopify.github.io/liquid/), and each of the items between double curly braces is a variable\. The pre\-annotation AWS Lambda function should provide an object named `taskInput` and that object's properties can be accessed as `{{ task.input.<property name> }}` in your template\.

## Your Intent Detection custom template<a name="sms-custom-templates-step2-demo2-your-template"></a>

In the starter template, there are two variables: the `task.input.labels` property in the `crowd-classifier` element opening tag and the `task.input.utterance` in the `classification-target` region's content\.

Unless you need to offer different sets of labels with different utterances, avoiding a variable and just using text will save processing time and creates less possibility of error\. The template used in this demonstration will remove that variable, but variables and filters like `to_json` are explained in more detail in the [`crowd-bounding-box` demonstration]() article\.

### Styling Your Elements<a name="sms-custom-templates-step2-demo2-instructions"></a>

Two parts of these custom elements that sometimes get overlooked are the `<full-instructions>` and `<short-instructions>` regions\. Good instructions generate good results\.

In the elements that include these regions, the `<short-instructions>` appear automatically in the "Instructions" pane on the left of the worker's screen\. The `<full-instructions>` are linked from the "View full instructions" link near the top of that pane\. Clicking the link opens a modal pane with more detailed instructions\.

You can not only use HTML, CSS, and JavaScript in these sections, you are encouraged to if you believe you can provide a strong set of instructions and examples that will help workers complete your tasks with better speed and accuracy\. 

**Example Try out a sample with JSFiddle**  
[https://jsfiddle.net/MTGT_Fiddle_Manager/bjc0y1vd/35/](https://jsfiddle.net/MTGT_Fiddle_Manager/bjc0y1vd/35/)  
 Try out an [example `<crowd-classifier>` task](https://jsfiddle.net/MTGT_Fiddle_Manager/bjc0y1vd/35/)\. The example is rendered by JSFiddle, therefore all the template variables are replaced with hard\-coded values\. Click the "View full instructions" link to see a set of examples with extended CSS styling\. You can fork the project to experiment with your own changes to the CSS, adding sample images, or adding extended JavaScript functionality\.

**Example : Final Customized Intent Detection Template**  
This uses the [example `<crowd-classifier>` task](https://jsfiddle.net/MTGT_Fiddle_Manager/bjc0y1vd/35/), but with a variable for the `<classification-target>`\. If you are trying to keep a consistent CSS design among a series of different labeling jobs, you can include an external stylesheet using a `<link rel...>` element the same way you'd do in any other HTML document\.  

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-classifier
    name="intent"
    categories="['buy', 'eat', 'watch', 'browse', 'leave']"
    header="Pick the most relevant intent expressed by the text below"
  >
    <classification-target>
      {{ task.input.source }}
    </classification-target>
    
    <full-instructions header="Emotion Classification Instructions">
      <p>In the statements and questions provided in this exercise, what category of action is the speaker interested in doing?</p>
          <table>
            <tr>
              <th>Example Utterance</th>
              <th>Good Choice</th>
            </tr>
            <tr>
              <td>When is the Seahawks game on?</td>
              <td>
                eat<br>
                <greenbg>watch</greenbg>
                <botchoice>browse</botchoice>
              </td>
            </tr>
            <tr>
              <th>Example Utterance</th>
              <th>Bad Choice</th>
            </tr>
            <tr>
              <td>When is the Seahawks game on?</td>
              <td>
                buy<br>
                <greenbg>eat</greenbg>
                <botchoice>watch</botchoice>
              </td>
            </tr>
          </table>
    </full-instructions>

    <short-instructions>
      What is the speaker expressing they would like to do next?
    </short-instructions>  
  </crowd-classifier>
</crowd-form>
<style>
  greenbg {
    background: #feee23;
    display: block;
  }

  table {
    *border-collapse: collapse; /* IE7 and lower */
    border-spacing: 0; 
  }

  th, tfoot, .fakehead {
    background-color: #8888ee;
    color: #f3f3f3;
    font-weight: 700;
  }

  th, td, tfoot {
      border: 1px solid blue;
  }

  th:first-child {
    border-radius: 6px 0 0 0;
  }

  th:last-child {
    border-radius: 0 6px 0 0;
  }

  th:only-child{
    border-radius: 6px 6px 0 0;
  }

  tfoot:first-child {
    border-radius: 0 0 6px 0;
  }

  tfoot:last-child {
    border-radius: 0 0 0 6px;
  }

  tfoot:only-child{
    border-radius: 6px 6px;
  }

  td {
    padding-left: 15px ;
    padding-right: 15px ;
  }

  botchoice {
    display: block;
    height: 17px;
    width: 490px;
    overflow: hidden;
    position: relative;
    background: #fff;
    padding-bottom: 20px;
  }

  botchoice:after {
    position: absolute;
    bottom: 0;
    left: 0;  
    height: 100%;
    width: 100%;
    content: "";
    background: linear-gradient(to top,
       rgba(255,255,255, 1) 55%, 
       rgba(255,255,255, 0) 100%
    );
    pointer-events: none; /* so the text is still selectable */
  }
</style>
```

**Example : Your manifest file**  
If you are preparing your manifest file manually for a text\-classification task like this, have your data formatted in the following manner\.  

```
{"source": "Roses are red"}
{"source": "Violets are Blue"}
{"source": "Ground Truth is the best"}
{"source": "And so are you"}
```

This differs from the manifest file used for the "[Demo Template: Annotation of Images with `crowd-bounding-box`](sms-custom-templates-step2-demo1.md)" demonstration in that `source-ref` was used as the property name instead of `source`\. The use of `source-ref` designates S3 URIs for images or other files that must be converted to HTTP\. Otherwise, `source` should be used like it is with the text strings above\.

## Your pre\-annotation Lambda function<a name="sms-custom-templates-step2-demo2-pre-lambda"></a>

As part of the job set\-up, provide the ARN of an AWS Lambda that can be called to process your manifest entries and pass them to the template engine\. 

This Lambda function is required to have one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\.

This applies to both your pre\-annotation and post\-annotation Lambdas\.

When you're using the console, if you have Lambdas that are owned by your account, a drop\-down list of functions meeting the naming requirements will be provided to choose one\.

In this very basic sample, where you have only one variable, it's primarily a pass\-through function\. Here's a sample pre\-labeling Lambda using Python 3\.7\.

```
import json

def lambda_handler(event, context):
    return {
        "taskInput":  event['dataObject']
    }
```

The `dataObject` property of the `event` contains the properties from a data object in your manifest\.

In this demonstration, which is a simple pass through, you just pass that straight through as the `taskInput` value\. If you add properties with those values to the `event['dataObject']` object, they will be available to your HTML template as Liquid variables with the format `{{ task.input.<property name> }}`\.

## Your post\-annotation Lambda function<a name="sms-custom-templates-step2-demo2-post-lambda"></a>

As part of the job set up, provide the ARN of an Lambda function that can be called to process the form data when a worker completes a task\. This can be as simple or complex as you want\. If you want to do answer\-consolidation and scoring as data comes in, you can apply the scoring or consolidation algorithms of your choice\. If you want to store the raw data for offline processing, that is an option\.

**Set permissions for your post\-annotation Lambda function**  
The annotation data will be in a file designated by the `s3Uri` string in the `payload` object\. To process the annotations as they come in, even for a simple pass through function, you need to assign `S3ReadOnly` access to your Lambda so it can read the annotation files\.  
In the Console page for creating your Lambda, scroll to the **Execution role** panel\. Select **Create a new role from one or more templates**\. Give the role a name\. From the **Policy templates** drop\-down, choose **Amazon S3 object read\-only permissions**\. Save the Lambda and the role will be saved and selected\.

The following sample is for Python 3\.7\.

```
import json
import boto3
from urllib.parse import urlparse

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
                        'result': new_annotation,
                        'labeledContent': dataset['dataObject']
                        }
                    }
                }
            }
            consolidated_labels.append(label)

    return consolidated_labels
```

## Your labeling job output<a name="sms-custom-templates-step2-demo2-job-output"></a>

The post\-annotation Lambda will often receive batches of task results in the event object\. That batch will be the `payload` object the Lambda should iterate through\.

You'll find the output of the job in a folder named after your labeling job in the target S3 bucket you specified\. It will be in a subfolder named `manifests`\.

For an intent detection task, the output in the output manifest will look a bit like the demo below\. The example has been cleaned up and spaced out to be easier for humans to read\. The actual output will be more compressed for machine reading\.

**Example : JSON in your output manifest**  

```
[
  {
    "datasetObjectId":"<Number representing item's place in the manifest>",
     "consolidatedAnnotation":
     {
       "content":
       {
         "<name of labeling job>":
         {     
           "workerId":"private.us-east-1.XXXXXXXXXXXXXXXXXXXXXX",
           "result":
           {
             "intent":
             {
                 "label":"<label chosen by worker>"
             }
           },
           "labeledContent":
           {
             "content":"<text content that was labeled>"
           }
         }
       }
     }
   },
  "datasetObjectId":"<Number representing item's place in the manifest>",
     "consolidatedAnnotation":
     {
       "content":
       {
         "<name of labeling job>":
         {     
           "workerId":"private.us-east-1.6UDLPKQZHYWJQSCA4MBJBB7FWE",
           "result":
           {
             "intent":
             {
                 "label": "<label chosen by worker>"
             }
           },
           "labeledContent":
           {
             "content": "<text content that was labeled>"
           }
         }
       }
     }
   },
     ...
     ...
     ...
]
```

This should help you create and use your own custom template\.