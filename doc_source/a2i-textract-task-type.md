# Use Amazon Augmented AI with Amazon Textract<a name="a2i-textract-task-type"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We do not recommend using this product in production environments\. | 

Amazon Textract enables you to add document text detection and analysis to your applications\. Amazon Augmented AI \(Amazon A2I\) directly integrated with Amazon Textract's `AnalyzeDocument` API operation\. You can use `AnalyzeDocument` to analyze a document for relationships between detected items\. When you add an Amazon A2I human review look to an `AnalyzeDocument` request, Amazon A2I will monitor the Amazon Textract results and send a document to a human when the inference confidence for key\-value pairs is low\. For example, if you want a human to review a specific key like "Full name:" and their associated input\-values you can create a trigger to start a human review anytime the key "Full name:" is detected or when the inference confidence for that key is low\. 

You can specify when Amazon Textract sends a task to a human worker for review when creating a human review workflow, or flow definition\. Within your flow definiton, you can specify thresholds for Identification confidence and Qualification confidence for specific form keys and for all form keys detected with low confidence\.
+ **Identification confidence**—The confidence score for key\-value pairs detected within a form\.
+ **Qualification confidence**—The confidence score for text contained within key and value respectively in a form\. 

In the image in the following section, **Full Name: Jane Doe** is the key\-value pair, and **Full Name** and **Jane Doe** are the key and value respectively\.

## A2I Textract Worker Console Preview<a name="a2i-textract-console-preview"></a>

When they're assigned a review task in an Amazon Textract workflow, workers might see UI similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i-textract-example.png)

You can customize this UI in the Amazon SageMaker console when you create your human review definition, or by creating and using a custom template\. To learn more, see [Create a Worker UI](a2i-instructions-overview.md)\.

## Integrate a Human Review into Amazon Textract<a name="a2i-create-textract-human-review"></a>

To integrate a human review into an Amazon Textract text detection and analysis job, you need to create a flow definition, and then use the Amazon Textract API to integrate that flow definition into your workflow\. To learn how to create a flow definition using the Amazon SageMaker console or Augmented AI API, see the following topics:
+ [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#create-human-review-console)
+ [Create a Flow Definition \(API\)](a2i-create-flow-definition.md#create-human-review-api)

After you've created your flow definition, see [Using Augmented AI with Amazon Textract](https://docs.aws.amazon.com/textract/latest/dg/a2i-textract.html) to learn how to integrate your flow definition into your Amazon Textract task\. 