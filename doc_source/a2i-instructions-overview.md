# Create and Manage Worker Task Templates<a name="a2i-instructions-overview"></a>

You can create a task UI for your workers by creating a *worker task template*\. A worker task template is an HTML file that is used to display your input data and instructions to help workers complete your task\.

For Amazon Rekognition or Amazon Textract task types, you can customize a pre\-made worker task template using a graphical user interface \(GUI\) and avoid interacting with HTML code\. For this option, use the instructions in [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#a2i-create-human-review-console) to create a human review workflow and customize your worker task template in the Amazon SageMaker console\. Once you create a template using these instructions, it will appear on the Worker task templates page of the [Augmented AI console](https://console.aws.amazon.com/a2i/)\.

If you are creating a human review workflow for a custom task type, you must create a *custom worker task template* using HTML code\. For more information, see [Create Custom Worker Task Template](a2i-custom-templates.md)\. 

If you create your template using HTML, you must use this template to generate an Amazon A2I *human task UI Amazon Resource Name \(ARN\)* in the Amazon A2I console\. This ARN has the following format: `arn:aws:sagemaker:<aws-region>:<aws-account-number>:human-task-ui/<template-name>`\. This ARN is associated with a worker task template resource that you can use in one or more human review workflows \(flow definitions\)\.

Generate a human task UI ARN using a worker task template by following the instructions found in [Create a Worker Task Template](a2i-worker-template-console.md#a2i-create-worker-template-console) or by using the [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html) API operation\.

**Topics**
+ [Create and Delete a Worker Task Templates](a2i-worker-template-console.md)
+ [Create Custom Worker Task Template](a2i-custom-templates.md)
+ [Creating Good Worker Instructions](a2i-creating-good-instructions-guide.md)