# Use APIs in Amazon Augmented AI<a name="a2i-api-references"></a>

You can create a human review workflow or a worker task template programmatically\. The APIs you use depend on whether you are creating a Amazon Rekognition, Amazon Textract, or custom task type\. This topic provides links to API reference documentation for each task type and programming task\.

The following APIs can be used with Augmented AI:

**Amazon Augmented AI**  
Use the Augmented AI API to start, stop, and delete human review loops\. You can also list all human review loops and return information about human review loops in your account\.  
Learn more about human review loop APIs in the [Amazon Augmented AI Runtime API Reference](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\.

**Amazon Rekognition**  
Use the **HumanLoopConfig** parameter of the [ DetectModerationLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectModerationLabels.html) API to trigger a human review workflow using Amazon Rekognition\.

**Amazon SageMaker**  
Use the Amazon SageMaker API to create a `FlowDefinition`, also known as a *human review workflow*\. You can also create a `HumanTaskUi`, or *worker task template*\.  
For more information, see the [ `CreateFlowDefinition`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html) or the [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html) API documentation\.

**Amazon Textract**  
Use the **HumanLoopConfig** parameter of the [AnalyzeDocument](https://docs.aws.amazon.com/textract/latest/dg/API_AnalyzeDocument.html) API to trigger a human review workflow using Amazon Textract\.

## Programmatic Walkthroughs<a name="amazon-augmented-ai-programmatic-walkthroughs"></a>

The following walkthroughs and tutorials provide example code and step\-by\-step instructions for creating human review workflows and worker task templates programmatically\.
+ [Create a Flow Definition \(API\)](a2i-create-flow-definition.md#a2i-create-human-review-api)
+ [Using Amazon Augmented AI with Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/a2i-rekognition.html) in the *Amazon Rekognition Developer Guide*
+ [Using Amazon Augmented AI with Amazon Textract AnalyzeDocument](https://docs.aws.amazon.com/textract/latest/dg/a2i-textract.html) in the *Amazon Textract Developer Guide*