# Using Amazon Augmented AI for Human Review<a name="a2i-use-augmented-ai-a2i-human-review-loops"></a>

When you use AI applications such as Amazon Rekognition, Amazon Textract, or your custom machine learning \(ML\) models, you can use Amazon Augmented AI to get human review of low\-confidence predictions or random prediction samples\.
<a name="what-is-amazon-augmented-ai-a2i"></a>
**What is Amazon Augmented AI?**  
Amazon Augmented AI \(Amazon A2I\) is a service that brings human review of ML predictions to all developers by removing the heavy lifting associated with building human review systems or managing large numbers of human reviewers\. 

Many ML applications require humans to review low\-confidence predictions to ensure the results are correct\. For example, extracting information from scanned mortgage application forms can require human review due to low\-quality scans or poor handwriting\. Building human review systems can be time\-consuming and expensive because it involves implementing complex processes or *workflows*, writing custom software to manage review tasks and results, and managing large groups of reviewers\.

Amazon A2I streamlines building and managing human reviews for ML applications\. Amazon A2I provides built\-in human review workflows for common ML use cases, such as content moderation and text extraction from documents\. You can also create your own workflows for ML models built on SageMaker or any other tools\. Using Amazon A2I, you can allow human reviewers to step in when a model is unable to make a high\-confidence prediction or to audit its predictions on an ongoing basis\. 
<a name="a2i-use-cases-intro"></a>
**Amazon A2I Use Case Examples**  
The following examples demonstrate how you can use Amazon A2I to integrate a human review loop into your ML application\. For each of these examples, you can find a Jupyter Notebook that demonstrates that workflow in [Use Cases and Examples Using Amazon A2I](a2i-task-types-general.md)\. 
+ **Use Amazon A2I with Amazon Textract** – Have humans review important key\-value pairs in single\-page documents or have Amazon Textract randomly sample and send documents from your dataset to humans for review\. 
+ **Use Amazon A2I with Amazon Rekognition** – Have humans review unsafe images for explicit adult or violent content if Amazon Rekognition returns a low\-confidence score, or have Amazon Rekognition randomly sample and send images from your dataset to humans for review\.
+ **Use Amazon A2I to review real\-time ML inferences** – Use Amazon A2I to review real\-time, low\-confidence inferences made by a model deployed to a SageMaker hosted endpoint and incrementally train your model using Amazon A2I output data\.
+ **Use Amazon A2I with Amazon Comprehend** – Have humans review Amazon Comprehend inferences about text data such as sentiment analysis, text syntax, and entity detection\.
+ **Use Amazon A2I with Amazon Transcribe** – Have humans review Amazon Transcribe transcriptions of video or audio files\. Use the results of transcription human review loops to create a custom vocabulary and improve future transcriptions of similar video or audio content\.
+ **Use Amazon A2I with Amazon Translate** – Have humans review low\-confidence translations returned from Amazon Translate\.
+ **Use Amazon A2I to review tabular data** – Use Amazon A2I to integrate a human review loop into an ML application that uses tabular data\.

![\[Amazon Augmented AI - How It Works\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/amazon-augmented-ai-how-it-works.png)

**Topics**
+ [Get Started with Amazon Augmented AI](a2i-getting-started.md)
+ [Use Cases and Examples Using Amazon A2I](a2i-task-types-general.md)
+ [Create a Human Review Workflow](a2i-create-flow-definition.md)
+ [Delete a Human Review Workflow](a2i-delete-flow-definition.md)
+ [Create and Start a Human Loop](a2i-start-human-loop.md)
+ [Delete a Human Loop](a2i-delete-human-loop.md)
+ [Create and Manage Worker Task Templates](a2i-instructions-overview.md)
+ [Monitor and Manage Your Human Loop](a2i-monitor-humanloop-results.md)
+ [Amazon A2I Output Data](a2i-output-data.md)
+ [Permissions and Security in Amazon Augmented AI](a2i-permissions-security.md)
+ [Use Amazon CloudWatch Events in Amazon Augmented AI](a2i-cloudwatch-events.md)
+ [Use APIs in Amazon Augmented AI](a2i-api-references.md)