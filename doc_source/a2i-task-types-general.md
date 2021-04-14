# Use Cases and Examples Using Amazon A2I<a name="a2i-task-types-general"></a>

You can use Amazon Augmented AI to incorporate a human review into your workflow for *built\-in task types*, Amazon Textract and Amazon Rekognition, or your own custom tasks using a *custom task type*\. 

When you create a human review workflow using one of the built\-in task types, you can specify conditions, such as confidence thresholds, that initiate a human review\. The service \(Amazon Rekognition or Amazon Textract\) creates a human loop on your behalf when these conditions are met and supplies your input data directly to Amazon A2I to send to human reviewers\. To learn more about the built\-in task types, use the following:
+ [Use Amazon Augmented AI with Amazon Textract](a2i-textract-task-type.md)
+ [Use Amazon Augmented AI with Amazon Rekognition](a2i-rekognition-task-type.md)

When you use a custom task type, you create and start a human loop using the Amazon A2I Runtime API\. Use the custom task type to incorporate a human review workflow with other AWS services or your own custom ML application\.
+ For more details, see [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)

The following table outlines a variety of Amazon A2I use cases that you can explore using SageMaker Jupyter Notebooks\. To get started with a Jupyter Notebook, use the instructions in [Use SageMaker Notebook Instance with Amazon A2I Jupyter Notebook](#a2i-task-types-notebook-demo)\. For more examples, see this [GitHub repository](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks)\. 


****  

| **Use Case** | **Description** | **Task Type** | 
| --- | --- | --- | 
|  [Use Amazon A2I with Amazon Textract](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Textract%20AnalyzeDocument.ipynb)  |  Have humans review single\-page documents to review important form key\-value pairs, or have Amazon Textract randomly sample and send documents from your dataset to humans for review\.   | Built\-in | 
| [Use Amazon A2I with Amazon Rekognition](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Rekognition%20DetectModerationLabels.ipynb) |  Have humans review unsafe images for explicit adult or violent content if Amazon Rekognition returns a low confidence score, or have Amazon Rekognition randomly sample and send images from your dataset to humans for review\.  |  Built\-in  | 
| [Use Amazon A2I with Amazon Comprehend](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Comprehend%20DetectSentiment.ipynb) |  Have humans review Amazon Comprehend inferences about text data such as sentiment analysis, text syntax, and entity detection\.  |  Custom  | 
| [Use Amazon A2I with Amazon Transcribe](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/A2I-Video-Transcription-with-Amazon-Transcribe.ipynb) |  Have humans review Amazon Transcribe transcriptions of video or audio files\. Use the results of transcription human review loops to create a custom vocabulary and improve future transcriptions of similar video or audio content\.  | Custom | 
| [Use Amazon A2I with Amazon Translate](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Amazon%20Translate.ipynb) |  Have humans review low\-confidence translations returned from Amazon Translate\.  |  Custom  | 
| [Use Amazon A2I to review real time ML inferences](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20A2I%20with%20Amazon%20SageMaker%20for%20object%20detection%20and%20model%20retraining.ipynb)  |  Use Amazon A2I to review real\-time, low\-confidence inferences made by a model deployed to a SageMaker hosted endpoint and incrementally train your model using Amazon A2I output data\.  |  Custom  | 
| [Use Amazon A2I to review tabular data](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(Amazon%20A2I)%20Integration%20with%20tabular%20data.ipynb) |  Use Amazon A2I to integrate a human review loop into an ML application that uses tabular data\.  |  Custom  | 

**Topics**
+ [Use SageMaker Notebook Instance with Amazon A2I Jupyter Notebook](#a2i-task-types-notebook-demo)
+ [Use Amazon Augmented AI with Amazon Textract](a2i-textract-task-type.md)
+ [Use Amazon Augmented AI with Amazon Rekognition](a2i-rekognition-task-type.md)
+ [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)

## Use SageMaker Notebook Instance with Amazon A2I Jupyter Notebook<a name="a2i-task-types-notebook-demo"></a>

For an end\-to\-end example that demonstrates how to integrate an Amazon A2I human review loop into a machine learning workflow, you can use a Jupyter Notebook from this [GitHub Repository](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks) in a SageMaker notebook instance\.

**To use an Amazon A2I custom task type sample notebook in an Amazon SageMaker notebook instance:**

1. If you do not have an active SageMaker notebook instance, create one by following the instructions in [Step 1: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

1. When your notebook instance is active, choose **Open JupyterLab** to the right of the notebook instance's name\. It may take a few moments for JupyterLab to load\. 

1. Choose the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squip_add_repo.png) icon to clone a GitHub repository into your workspace\. 

1. Enter the [amazon\-a2i\-sample\-jupyter\-notebooks](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks) repository HTTPS URL\. 

1. Choose **CLONE**\.

1. Open the notebook that you would like to run\. 

1. Follow the instructions in the notebook to configure your human review workflow and human loop and run the cells\. 

1. To avoid incurring unnecessary charges, when you are done with the demo, stop and delete your notebook instance in addition to any Amazon S3 buckets, IAM roles, and CloudWatch Events resources created during the walkthrough\.