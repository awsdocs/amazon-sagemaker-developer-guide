# Use Amazon Augmented AI with Custom Task Types<a name="a2i-task-types-custom"></a>

You can use Amazon Augmented AI \(Amazon A2I\) to incorporate a human review \(human loop\) into your custom machine learning workflow\. This is known as a *custom task type*\.

**To create a human loop using a flow definition, integrate it into your application, and monitor the results**

1. Complete the Amazon A2I [Prerequisites](a2i-getting-started.md#a2i-getting-started-prerequisites)\. Note the following: 
   + The path the Amazon Simple Storage Service \(Amazon S3\) bucket or buckets where you will store your input and output data\. 
   + The Amazon Resource Name \(ARN\) of an AWS Identity and Access Management \(IAM\) role with required permissions attached\. 
   + \(Optional\) If you plan to use a private workforce, the ARN of your workforce\. 

1. Using HTML elements, create a custom worker template which Amazon A2I uses to generate your worker task UI\. To learn how to create a custom template, see [Create Custom Worker Task Template](a2i-custom-templates.md)\. 

1. Use the custom worker template from Step 2 to generate a worker task template in the Amazon SageMaker console\. To learn how, see [Create a Worker Task Template](a2i-worker-template-console.md#a2i-create-worker-template-console)\.

   In the next Step you will create a flow definition:
   + If you want to create a flow definition using the SageMaker API, note the ARN of this worker task template for the next step\.
   + If you are creating a flow definition using the console, your template will automatically appear in **Worker task template** section when you choose **Create human review workflow**\.

1. When creating your flow definition, provide the path to your S3 buckets, your IAM role ARN, and your worker template\. 
   + Learn how to create a flow definition using the SageMaker `CreateFlowDefinition` API: [Create a Flow Definition \(API\)](a2i-create-flow-definition.md#a2i-create-human-review-api)\. 
   + Learn how to create a flow definition using the SageMaker console: [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#a2i-create-human-review-console)\.

1. Configure your human loop using the [Amazon A2I Runtime API](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/Welcome.html)\. To learn how, see [Create and Start a Human Loop](a2i-start-human-loop.md)\. 

1. To control when human reviews are initiated in your application, specify conditions under which `StartHumanLoop` is called in your application\. Human loop activation conditions, such as confidence thresholds that trigger the human loop, are not available when using Amazon A2I with custom task types\. Every `StartHumanLoop` invocation results in a human review\.

Once you have started a human loop, you can manage and monitor your loops using the Amazon Augmented AI Runtime API and Amazon EventBridge \(also known as Amazon CloudWatch Events\)\. To learn more, see [Monitor and Manage Your Human Loop](a2i-monitor-humanloop-results.md)\.

## End\-to\-end Demo Using Augmented AI in a custom ML workflow<a name="a2i-task-types-custom-notebook-demo"></a>

For an end\-to\-end example that demonstrates how to integrate a human review loop into a custom machine learning workflow using Augmented AI, you can use a Jupyter notebook from this [GitHub Repository](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks) in a SageMaker Notebook instance\. 
+ Use Amazon **Amazon Augmented AI \(Amazon A2I\) integration with Amazon Comprehend \[Example\]** in the following procedure to integrate a human review look into an Amazon Comprehend sentimental anlaysis workflow\. 
+ Use Amazon **Amazon Augmented AI \(Amazon A2I\) integration with Amazon SageMaker Hosted Endpoint \[Example\]** in the following procedure to learn how to integrate a human review loop into a machine learning workflow that uses a hosted SageMaker endpoint to deliver real\-time predictions\.

**To use an Augmented AI custom task type sample notebook in an Amazon SageMaker Notebook**

1. If you do not have an active SageMaker Notebook instance, create one by following the instructions in [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

1. When your Notebook instance is active, choose **Open JupyterLab** to the right of the Notebook instance's name\. It may take a few moments for JupyterLab to load\. 

1. Choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squip_add_repo.png) icon to clone a GitHub repository into your workspace\. 

1. Enter the [amazon\-a2i\-sample\-jupyter\-notebooks](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks) repository HTTPS URL\. 

1. Choose **CLONE**\.

1. Open the notebook that you would like to run\. 

1. Follow the instructions in the notebook to configure your flow definition and human loop and run the cells\. 

1. To avoid incurring unnecessary charges, when you are done with the demo stop and delete your notebook instance in addition to any Amazon S3 buckets, IAM roles, and CloudWatch Events resources created during the walkthrough\.