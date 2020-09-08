# Use Amazon Augmented AI with Amazon Textract<a name="a2i-textract-task-type"></a>

Amazon Textract enables you to add document text detection and analysis to your applications\. Amazon Augmented AI \(Amazon A2I\) directly integrated with Amazon Textract's `AnalyzeDocument` API operation\. You can use `AnalyzeDocument` to analyze a document for relationships between detected items\. When you add an Amazon A2I human review loop to an `AnalyzeDocument` request, Amazon A2I monitors the Amazon Textract results and sends a document to one or more human workers for review when the conditions specified in your flow definition are met\. For example, if you want a human to review a specific key like "Full name:" and their associated input\-values you can create a trigger to start a human review anytime the key "Full name:" is detected or when the inference confidence for that key falls within a range that you specify\. 

You can specify when Amazon Textract sends a task to a human worker for review when creating a human review workflow, or flow definition by specifying *activation conditions*\. 

You can set the following activation conditions when using the Amazon Textract task type:
+ Trigger a human review for specific form keys based on the form key confidence score\. 
+ Trigger a human review when specific form keys are missing\. 
+ Trigger human review for all form keys identified by Amazon Textract with confidence scores in a specified range\.
+ Randomly send a sample of forms to humans for review\.

When your activation condition depends on form key confidence scores, you can use two types of prediction\-confidence to trigger human loops:
+ **Identification confidence** – The confidence score for key\-value pairs detected within a form\.
+ **Qualification confidence** – The confidence score for text contained within key and value respectively in a form\.

In the image in the following section, **Full Name: Jane Doe** is the key\-value pair, and **Full Name** and **Jane Doe** are the key and value respectively\.

You can set these activation conditions using the Amazon SageMaker console when you create a human review workflow, or by creating a JSON for human loop activation conditions and specifying this as input in the `HumanLoopActivationConditions` parameter of `CreateFlowDefinition` API operation\. To learn how specify activation conditions in JSON format, see [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md) and [Use Human Loop Activation Conditions JSON Schema with Amazon Textract](a2i-json-humantaskactivationconditions-textract-example.md)\.

**Note**  
When using Augmented AI with Amazon Textract, create Augmented AI resources in the same AWS Region you use to call `AnalyzeDocument`\. 

## Get Started: Integrate a Human Review into an Amazon Textract Analyze Document Job<a name="a2i-create-textract-human-review"></a>

To integrate a human review into an Amazon Textract text detection and analysis job, you need to create a flow definition, and then use the Amazon Textract API to integrate that flow definition into your workflow\. To learn how to create a flow definition using the SageMaker console or Augmented AI API, see the following topics:
+ [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#a2i-create-human-review-console)
+ [Create a Flow Definition \(API\)](a2i-create-flow-definition.md#a2i-create-human-review-api)

After you've created your flow definition, see [Using Augmented AI with Amazon Textract](https://docs.aws.amazon.com/textract/latest/dg/a2i-textract.html) to learn how to integrate your flow definition into your Amazon Textract task\. 

## End\-to\-end Demo Using Amazon Textract and Augmented AI<a name="a2i-task-types-textract-notebook-demo"></a>

For an end\-to\-end example that demonstrates how to use Amazon Textract with Augmented AI, you can use [Amazon Augmented AI \(Amazon A2I\) integration with Amazon Textract's Analyze Document \[Example\]](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Textract%20AnalyzeDocument.ipynb) in a SageMaker Notebook instance\. 

**To use Amazon Textract with Augmented AI using a Amazon SageMaker Notebook**

1. If you do not have an active SageMaker Notebook instance, create one by following the instructions in [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

1. When your Notebook instance is active, choose **Open JupyterLab** to the right of the Notebook instance's name\. It may take a few moments for JupyterLab to load\. 

1. Choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squip_add_repo.png) icon to clone a GitHub repository into your workspace\. 

1. Enter the [amazon\-a2i\-sample\-jupyter\-notebooks](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks) repository HTTPS URL\. 

1. Choose **CLONE**\.

1. Open the notebook **Amazon Augmented AI \(Amazon A2I\) integration with Amazon Textract's Analyze Document \[Example\]**\. 

1. Follow the instructions in the notebook to configure your flow definition and human loop and run the cells\. 

1. To avoid incurring unnecessary charges, when you are done with the demo stop and delete your notebook instance in addition to any Amazon S3 buckets, IAM roles, and CloudWatch Events resources created during the walkthrough\.

## A2I Textract Worker Console Preview<a name="a2i-textract-console-preview"></a>

When they're assigned a review task in an Amazon Textract workflow, workers might see UI similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i-textract-example.png)

You can customize this UI in the SageMaker console when you create your human review definition, or by creating and using a custom template\. To learn more, see [Create and Manage Worker Task Templates](a2i-instructions-overview.md)\.