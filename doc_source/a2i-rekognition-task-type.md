# Use Amazon Augmented AI with Amazon Rekognition<a name="a2i-rekognition-task-type"></a>

Amazon Rekognition makes it easy to add image analysis to your applications\. The Amazon Rekognition `DetectModerationLabels`API operation is directly integrated with Amazon A2I so that you can easily create a human loop to review unsafe images, such as explicit adult or violent content\. You can use `DetectModerationLabels` to configure a human loop using a flow definition ARN\. This enables Amazon A2I to analyze predictions made by Amazon Rekognition and sent results to a human for review they meet the conditions set in your flow definition\.

You can set the following activation conditions when using the Amazon Rekognition task type:
+ Trigger human review for labels identified by Amazon Rekognition based on the label confidence score\.
+ Randomly send a sample of images to humans for review\.

You can set these activation conditions using the Amazon SageMaker console when you create a human review workflow, or by creating a JSON for human loop activation conditions and specifying this as input in the `HumanLoopActivationConditions` parameter of `CreateFlowDefinition` API operation\. To learn how specify activation conditions in JSON format, see [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md) and [Use Human Loop Activation Conditions JSON Schema with Amazon Rekognition](a2i-json-humantaskactivationconditions-rekognition-example.md)\.

**Note**  
When using Augmented AI with Amazon Rekognition, create Augmented AI resources in the same AWS Region you use to call `DetectModerationLabels`\. 

## Get Started: Integrate a Human Review into an Amazon Rekognition Image Moderation Job<a name="a2i-create-rekognition-human-review"></a>

To integrate a human review into an Amazon Rekognition, see the following topics:
+ [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#a2i-create-human-review-console)
+ [Create a Flow Definition \(API\)](a2i-create-flow-definition.md#a2i-create-human-review-api)

After you've created your flow definition, see [Using Augmented AI with Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/a2i-rekognition.html) to learn how to integrate your flow definition into your Amazon Rekognition task\. 

## End\-to\-end Demo Using Amazon Rekognition and Augmented AI<a name="a2i-task-types-rekognition-notebook-demo"></a>

For an end\-to\-end example that demonstrates how to use Amazon Rekognition with Augmented AI, you can use [Amazon Augmented AI \(Amazon A2I\) integration with Amazon Rekognition \[Example\]](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Rekognition%20DetectModerationLabels.ipynb) in a SageMaker Notebook instance\. 

**To use Amazon Textract with Augmented AI using a Amazon SageMaker Notebook**

1. If you do not have an active SageMaker Notebook instance, create one by following the instructions in [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

1. When your Notebook instance is active, choose **Open JupyterLab** to the right of the Notebook instance's name\. It may take a few moments for JupyterLab to load\. 

1. Select the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squip_add_repo.png) icon to clone a GitHub repository into your workspace\. 

1. Enter the [amazon\-a2i\-sample\-jupyter\-notebooks](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks) repository HTTPS URL\. 

1. Choose **CLONE**\.

1. Open the notebook **Amazon Augmented AI \(Amazon A2I\) integration with Amazon Rekognition \[Example\]**\. 

1. Follow the instructions in the notebook to configure your flow definition and human loop and run the cells\. 

1. To avoid incurring unnecessary charges, when you are done with the demo stop and delete your notebook instance in addition to any Amazon S3 buckets, IAM roles, and CloudWatch Events resources created during the walkthrough\.

## A2I Rekognition Worker Console Preview<a name="a2i-rekognition-console-preview"></a>

When they're assigned a review task in an Amazon Rekognition workflow, workers might see UI similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i-rekognition-example.png)

You can customize this interface in the SageMaker console when you create your human review definition, or by creating and using a custom template\. To learn more, see [Create and Manage Worker Task Templates](a2i-instructions-overview.md)\.