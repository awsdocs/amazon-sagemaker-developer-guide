# Core Components of Amazon A2I<a name="a2i-getting-started-core-components"></a>

Review the following terms to familiarize yourself with the core components of Amazon A2I\. 

## Task Types<a name="a2i-task-type-get-started"></a>

The the AI/ML workflow into which you integrate Amazon A2I defines an Amazon A2I *task type*\. 

Amazon A2I supports two *built\-in task types*: Amazon Textract and Amazon Rekognition, and a *custom task type*\. 

You can use a custom task type to integrate Amazon A2I with other AWS services like Amazon Comprehend, Amazon Transcribe, and Amazon Translate, as well as your own custom machine learning workflows\. To learn more, see [Use Cases and Examples using Amazon A2I](a2i-task-types-general.md)\.

## Human review workflow \(flow definition\)<a name="a2i-getting-started-human-review-workflow"></a>

You use a human review workflow to specify your human *work team*, to set up your worker UI using a *worker task template*, and to provide information about how workers should complete the review task\. 

For built\-in task types, you also use the human review workflow to identify the conditions under which a human loop is triggered\. For example, Amazon Rekognition can perform image content moderation using machine learning\. You can use the human review workflow to specify that an image will be sent to a human for content moderation review if Amazon Rekognition's confidence is too low\.

You can use a human review workflow to create multiple human loops\.

You can create a flow definition in the SageMaker console or with the SageMaker API\. To learn more about both of these options, see [Create a Human Review Workflow](a2i-create-flow-definition.md)\.

**Work Team**  
A *work team* is a group of human workers to whom you send your human review tasks\.

When you create a human review workflow, you specify a single work team\. 

Your work team can come from the [Amazon Mechanical Turk workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-public.html), a [vendor\-managed workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management-vendor.html), or your own [private workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-private.html)\. When you use the private workforce, you can create multiple work teams\. Each work team can be used in multiple human review workflows\. To learn how to create a workforce and work teams, see [Create and Manage Workforces](sms-workforce-management.md)\.

**Worker Task Template and Human Task UI**  
You use a *worker task template* to create a worker UI that is used for your human review tasks\. The worker UI is referred to as a *human task ui*\.

The human task UI displays your input data, such as documents or images, and instructions to workers\. It also provides interactive tools that the worker uses to complete your tasks\. 

For built\-in task types, you must use a the Amazon A2I worker task template provided for that task type\.

## Human Loops<a name="a2i-getting-started-human-loop"></a>

A *human loop* is used to create a single human review job\. For each human review job, you can choose the number of workers that are sent a *task* to review a single data object\. For example, if you set the number of workers per object to 3 for an image classification labeling job, three workers will classify each input image\. Increasing the number of workers per object can improve label accuracy\.

A human loop is created using a human review workflow as follows:
+ For built\-in task types, the conditions specified in the human review workflow determine when the human loop is created\.
+ Human review tasks are sent to the work team specified in the human review workflow\. 
+ The worker task template specified in the human review workflow is used to render the human task UI\. 

**When do human loops get created?**

When you use one of the *built\-in task types*, the corresponding AWS service creates and starts a human loop on your behalf when the conditions specified in your human review workflow are met\. For example:
+ When you use Augmented AI with Amazon Textract, you can integrate Amazon A2I into a document review task using the API operation `AnalyzeDocument`\. A human loop will get created everytime Amazon Textract returns inferences about key value pairs that meet the conditions you specify in your human review workflow\. 
+ When you use Augmented AI with Amazon Rekognition, you can integrate Amazon A2I into an image moderation task using the API operation `DetectModerationLabels`\. A human loop will get created everytime Amazon Rekognition returns inferences about image content that meet the conditions you specify in your human review workflow\.

When using a *custom task type*, you start a human loop using the [Amazon Augmented AI Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. When you call `StartHumanLoop` in your custom application, a task is sent to human reviewers\. 

To learn how to create and start a human loop, see [Create and Start a Human Loop](a2i-start-human-loop.md)\.

To generate these resources and create a human review workflow, Amazon A2I integrates multiple APIs including the Amazon Augmented AI Runtime Model, the SageMaker APIs, and APIs associated with your task type\. To learn more, see [Use APIs in Amazon Augmented AIAPI References](a2i-api-references.md)\.