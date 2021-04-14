# Use Amazon Augmented AI with Amazon Textract<a name="a2i-textract-task-type"></a>

Amazon Textract enables you to add document text detection and analysis to your applications\. Amazon Augmented AI \(Amazon A2I\) directly integrates with Amazon Textract's `AnalyzeDocument` API operation\. You can use `AnalyzeDocument` to analyze a document for relationships between detected items\. When you add an Amazon A2I human review loop to an `AnalyzeDocument` request, Amazon A2I monitors the Amazon Textract results and sends a document to one or more human workers for review when the conditions specified in your flow definition are met\. For example, if you want a human to review a specific key like `Full name:` and their associated input values, you can create an activation condition that starts a human review any time the `Full name:` key is detected or when the inference confidence for that key falls within a range that you specify\. 

The following image depicts the Amazon A2I built\-in workflow with Amazon Textract\. On the left, the resources that are required to create an Amazon Textract human review workflow are depicted: and Amazon S3 bucket, activation conditions, a worker task template, and a work team\. These resources are used to create a human review workflow, or flow definition\. An arrow points right to the next step in the workflow: using Amazon Textract to configure a human loop with the human review workflow\. A second arrow points right from this step to the step in which activation conditions specified in the human review workflow are met\. This initiates the creation of a human loop\. On the right of the image, the human loop is depicted in three steps: 1\) the worker UI and tools are generated and the task is made available to workers,2\) workers review input data, and finally, 3\) results are saved in Amazon S3\.

![\[Use Amazon Augmented AI with Amazon Textract\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i/diagrams/product-page-diagram_A21-Components_Textract@2x.png)

You can specify when Amazon Textract sends a task to a human worker for review when creating a human review workflow or flow definition by specifying *activation conditions*\. 

You can set the following activation conditions when using the Amazon Textract task type:
+ Initiate a human review for specific form keys based on the form key confidence score\. 
+ Initiate a human review when specific form keys are missing\. 
+ Initiate human review for all form keys identified by Amazon Textract with confidence scores in a specified range\.
+ Randomly send a sample of forms to humans for review\.

When your activation condition depends on form key confidence scores, you can use two types of prediction confidence to initiate human loops:
+ **Identification confidence** – The confidence score for key\-value pairs detected within a form\.
+ **Qualification confidence** – The confidence score for text contained within key and value in a form\.

In the image in the following section, **Full Name: Jane Doe** is the key\-value pair, **Full Name** is the key, and **Jane Doe** is the value\.

You can set these activation conditions using the Amazon SageMaker console when you create a human review workflow, or by creating a JSON for human loop activation conditions and specifying this as input in the `HumanLoopActivationConditions` parameter of `CreateFlowDefinition` API operation\. To learn how specify activation conditions in JSON format, see [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md) and [Use Human Loop Activation Conditions JSON Schema with Amazon Textract](a2i-json-humantaskactivationconditions-textract-example.md)\.

**Note**  
When using Augmented AI with Amazon Textract, create Augmented AI resources in the same AWS Region you use to call `AnalyzeDocument`\. 

## Get Started: Integrate a Human Review into an Amazon Textract Analyze Document Job<a name="a2i-create-textract-human-review"></a>

To integrate a human review into an Amazon Textract text detection and analysis job, you need to create a flow definition, and then use the Amazon Textract API to integrate that flow definition into your workflow\. To learn how to create a flow definition using the SageMaker console or Augmented AI API, see the following topics:
+ [Create a Human Review Workflow \(Console\)](a2i-create-flow-definition.md#a2i-create-human-review-console)
+ [Create a Human Review Workflow \(API\)](a2i-create-flow-definition.md#a2i-create-human-review-api)

After you've created your flow definition, see [Using Augmented AI with Amazon Textract](https://docs.aws.amazon.com/textract/latest/dg/a2i-textract.html) to learn how to integrate your flow definition into your Amazon Textract task\. 

## End\-to\-End Example Using Amazon Textract and Amazon A2I<a name="a2i-task-types-textract-notebook-demo"></a>

For an end\-to\-end example that demonstrates how to use Amazon Textract with Amazon A2I using the console, see [Tutorial: Get Started in the Amazon A2I Console](a2i-get-started-console.md)\.

To learn how to use the Amazon A2I API to create and start a human review, you can use [Amazon Augmented AI \(Amazon A2I\) integration with Amazon Textract's Analyze Document \[Example\]](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Textract%20AnalyzeDocument.ipynb) in a SageMaker Notebook instance\. To get started, see [Use SageMaker Notebook Instance with Amazon A2I Jupyter Notebook](a2i-task-types-general.md#a2i-task-types-notebook-demo)\.

## A2I Textract Worker Console Preview<a name="a2i-textract-console-preview"></a>

When they're assigned a review task in an Amazon Textract workflow, workers might see a user interface similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i-textract-example.png)

You can customize this interface in the SageMaker console when you create your human review definition, or by creating and using a custom template\. To learn more, see [Create and Manage Worker Task Templates](a2i-instructions-overview.md)\.