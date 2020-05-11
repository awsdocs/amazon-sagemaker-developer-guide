# Use Task Types<a name="a2i-task-types-general"></a>

You can use Augmented AI to incorporate a human review into your workflow for *built\-in task types*, Amazon Textract and Amazon Rekognition, or your own custom tasks\. 

When you create a flow definition using one of the built\-in task types, you will be able to specify conditions, such as confidence thresholds, that will trigger a human review\. For example, you can specify confidence thresholds for form keys in a document, or inference confidence for an image\. These services create a human loop on your behalf using Amazon Augmented AI Runtime API when these conditions are met, and supply your input data directly to Amazon A2I to send to human reviewers\. 

When you use a custom task type, you create and start a human loop using the Amazon Augmented AI Runtime API\. For more details, see [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)\. Use the custom task type to incorporate a human review workflow with other AWS service, or your own custom ML application\.

The topics in this section provide an overview of these three task types and examples of worker task templates that Amazon A2I provides for Amazon Textract and Amazon Rekognition task types\. You specify your task type when creating a flow definition\.

**Topics**
+ [Use Amazon Augmented AI with Amazon Textract](a2i-textract-task-type.md)
+ [Use Amazon Augmented AI with Amazon Rekognition](a2i-rekognition-task-type.md)
+ [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)