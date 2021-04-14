# Use Amazon Augmented AI with Custom Task Types<a name="a2i-task-types-custom"></a>

You can use Amazon Augmented AI \(Amazon A2I\) to incorporate a human review \(human loop\) into *any* machine learning workflow using the *custom task type*\. This options gives you the most flexibility to customize the conditions under which your data objects are sent to humans for review, as well as the look and feel of your worker user interface\.

When you use a custom task type, you create a custom human review workflow and specify the conditions under which a data object is sent for human review directly in your application\. 

The following image depicts the Amazon A2I custom workflow\. A custom ML model is used to generate predictions\. The client application filters these predictions using user\-defined criteria and determines if a human review is required\. If so, these predictions are sent to Amazon A2I for human review\. Amazon A2I collects the results of human review in Amazon S3, which can access by the client application\. If the filter determines that no human review is needed, predictions can be fed directly to the client application\. 

![\[Use Amazon Augmented AI with Custom Task Types\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i/diagrams/product-page-diagram_A2I-Components_Custom@2x.png)

Use the procedures on this page to learn how to integrate Amazon A2I into any machine learning workflow using the custom task type\. 

**Create a human loop using a flow definition, integrate it into your application, and monitor the results**

1. Complete the Amazon A2I [Prerequisites to Using Augmented AI](a2i-getting-started-prerequisites.md)\. Note the following: 
   + The path to the Amazon Simple Storage Service \(Amazon S3\) bucket or buckets where you store your input and output data\. 
   + The Amazon Resource Name \(ARN\) of an AWS Identity and Access Management \(IAM\) role with required permissions attached\. 
   + \(Optional\) The ARN of your private workforce, if you plan to use one\. 

1. Using HTML elements, create a custom worker template which Amazon A2I uses to generate your worker task UI\. To learn how to create a custom template, see [Create Custom Worker Task Templates](a2i-custom-templates.md)\. 

1. Use the custom worker template from step 2 to generate a worker task template in the Amazon SageMaker console\. To learn how, see [Create a Worker Task Template](a2i-worker-template-console.md#a2i-create-worker-template-console)\.

   In the next step, you create a flow definition:
   + If you want to create a flow definition using the SageMaker API, note the ARN of this worker task template for the next step\.
   + If you are creating a flow definition using the console, your template automatically appears in **Worker task template** section when you choose **Create human review workflow**\.

1. When creating your flow definition, provide the path to your S3 buckets, your IAM role ARN, and your worker template\. 
   + To learn how to create a flow definition using the SageMaker `CreateFlowDefinition` API, see [Create a Human Review Workflow \(API\)](a2i-create-flow-definition.md#a2i-create-human-review-api)\. 
   + To learn how to create a flow definition using the SageMaker console, see [Create a Human Review Workflow \(Console\)](a2i-create-flow-definition.md#a2i-create-human-review-console)\.

1. Configure your human loop using the [Amazon A2I Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. To learn how, see [Create and Start a Human Loop](a2i-start-human-loop.md)\. 

1. To control when human reviews are initiated in your application, specify conditions under which `StartHumanLoop` is called in your application\. Human loop activation conditions, such as confidence thresholds that initiate the human loop, are not available when using Amazon A2I with custom task types\. Every `StartHumanLoop` invocation results in a human review\.

Once you have started a human loop, you can manage and monitor your loops using the Amazon Augmented AI Runtime API and Amazon EventBridge \(also known as Amazon CloudWatch Events\)\. To learn more, see [Monitor and Manage Your Human Loop](a2i-monitor-humanloop-results.md)\.

## End\-to\-end Tutorial Using Amazon A2I Custom Task Types<a name="a2i-task-types-custom-notebook-demo"></a>

For an end\-to\-end examples that demonstrates how to integrate Amazon A2I into a variety of ML workflows, see the table in [Use Cases and Examples Using Amazon A2I](a2i-task-types-general.md)\. To get started using one of these notebooks, see [Use SageMaker Notebook Instance with Amazon A2I Jupyter Notebook](a2i-task-types-general.md#a2i-task-types-notebook-demo)\.