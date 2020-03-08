# Get Started with Amazon Augmented AI<a name="a2i-getting-started"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We do not recommend using this product in production environments\. | 

Amazon Augmented AI \(Amazon A2I\) helps you integrate human judgment into AI/ML workflows\. With Amazon A2I, you can let AI handle straight\-forward data and invoke human reviewers only when their skills are needed\. 

The the AI/ML workflow that you integrate Amazon A2I into defines an Amazon A2I *task type*\. Amazon A2I supports two *built\-in task types*: Amazon Textract and Amazon Rekognition, and a *custom task type*\. The built\-in task types integrate Amazon A2I with [Amazon Textract]()'s `AnalyzeDocument` API operation and [Amazon Rekognition]()'s `DetectModerationLabels` API operation to incorporate a human review workflow when inference confidence is low for a given object \(for example, for an image or text in a document\)\. The custom task type allows you to use Amazon A2I in custom machine learning applications\. For more information about task types, see [Use Task Types](a2i-task-types-general.md)\.

To incorporate Amazon A2I into your data labeling workflow for all task types, you need three resources: 
+ A *worker task template* to create a worker UI\. The worker UI displays your input data, such as documents or images, and instructions to workers\. It also provides interactive tools that the worker uses to complete your tasks\. For more information, see [Create a Worker UI](a2i-instructions-overview.md)\.
+ A *human review workflow*, also referred to as a *flow definition*\. You use the flow definition to configure your human workforce and provide information about how to accomplish the labeling task\. For built\-in task types, you also use the flow definition to identify the conditions under which a review human loop is triggered\. For example, Amazon Rekognition can perform image content moderation using machine learning\. You can use the flow definition to specify that an image will be sent to a human for content moderation review if Amazon Rekognition's confidence is too low\. You can create a flow definition in the Amazon SageMaker console or with the Amazon SageMaker API\. To learn more about both of these options, see [Create a Flow Definition](a2i-create-flow-definition.md)\.
+ A *human loop* to start your human review workflow\. When you use one of the built\-in task types, the corresponding AWS service creates and starts a human loop on your behalf when the conditions specified in your flow definition are met or for each object if no conditions were specified\. When a human loop is triggered, human review tasks are sent to the workers as specified in the flow definition\. 

  When using a custom task type, you start a human loop using the [Amazon Augmented AI Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. When you call `StartHumanLoop` in your custom application, a task is sent to human reviewers\. 

  To learn how to create and start a human loop, see [Create and Start a Human Loop](a2i-start-human-loop.md)\.

To generate these resources and create a human review workflow, Amazon A2I integrates multiple APIs including the Amazon Augmented AI Runtime Model, the Amazon SageMaker APIs, and APIs associated with your task type\. To learn more, see [Use APIs in Amazon Augmented AIAPI References](a2i-api-references.md)\.

## Prerequisites<a name="a2i-getting-started-prerequisites"></a>

You can create a Amazon A2I human review workflow using both the Amazon SageMaker console and an API\. To create a human review workflow, you need the following: 
+ One or more Amazon S3 buckets in the same AWS Region as the workflow for your input and output data\. To create a bucket, follow the instructions in [ Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. 
+ An IAM role with required permissions to create a human review workflow and an IAM user or role with permission to access Augmented AI\. For more information, see [Permissions and Security in Amazon Augmented AIPermissions and Security](a2i-permissions-security.md)\.
+ You're prompted to choose a public, private, or vendor workforce for your human review workflows\. If you plan to use a private workforce, you need to set one up ahead of time in the same AWS Region as your Amazon A2I workflow\. To learn more about these workforce types, see [Create and Manage Workforces](sms-workforce-management.md)\.
**Important**  
Use the Amazon Mechanical Turk workforce only to process data that is public or has been stripped of all sensitive information\. Do not share confidential information, personal information, or protected health information with this workforce\. Do not use the Amazon Mechanical Turk workforce when you use Amazon Augmented AI in conjunction with other AWS HIPAA\-eligible services, such as Amazon Textract and Amazon Rekognition\.

## Next Steps<a name="next-steps-a2i"></a>

If you're new to Amazon A2I and are integrating a human review workflow with an Amazon Rekognition or Amazon Textract task, we recommend that you start by creating a human review workflow using the console\. For more information, see [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#create-human-review-console)\. 

If you are creating a human review workflow for a custom machine learning task, start by following the steps in [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)\. 