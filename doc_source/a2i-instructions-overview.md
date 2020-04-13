# Create a Worker UI<a name="a2i-instructions-overview"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We do not recommend using this product in production environments\. | 

You can create a task UI for your workers by creating a *worker task template* with detailed instructions on how to complete your task\. Depending on your task type, this can be done directly in the Amazon SageMaker console or by using a custom template\. 
+ If you are creating a human review workflow for Amazon Textract or Amazon Rekognition tasks, follow the instructions in [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#create-human-review-console) to create your worker template using the default templates provided by Amazon A2I\.
+ If you are adding a human review workflow to a custom task type, or want to create a custom worker UI using HTML and CSS elements:

  1. Create a custom worker template using the instructions found in [Create Custom Worker Templates](a2i-custom-templates.md)\. 

  1. Generate an Amazon Resource Name \(ARN\) to use this template in your flow definition\. Generate a worker task template ARN using the instructions found in [Create a Custom Worker Template \(Console\)](create-worker-template-console.md) or by using the [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html) API operation\. 

**Topics**
+ [Create a Custom Worker Template \(Console\)](create-worker-template-console.md)
+ [Create Custom Worker Templates](a2i-custom-templates.md)
+ [Creating Good Worker Instructions](a2i-creating-good-instructions-guide.md)