# Using Amazon Augmented AI for Human Review<a name="a2i-use-augmented-ai-a2i-human-review-loops"></a>

When you use AI applications such as Amazon Rekognition, Amazon Textract, or your custom machine learning \(ML\) models you can use Amazon Augmented AI to get human review of low confidence or a random sample of predictions\.
<a name="what-is-amazon-augmented-ai-a2i"></a>
**What is Amazon Augmented AI?**  
Amazon Augmented AI \(Amazon A2I\) makes it easy to build the workflows required for human review of ML predictions\. Amazon A2I brings human review to all developers, removing the undifferentiated heavy lifting associated with building human review systems or managing large numbers of human reviewers\.

Many machine learning applications require humans to review low\-confidence predictions to ensure the results are correct\. For example, extracting information from scanned mortgage application forms can require human review in some cases due to low\-quality scans or poor handwriting\. But building human review systems can be time\-consuming and expensive because it involves implementing complex processes or *workflows*, writing custom software to manage review tasks and results, and in many cases, managing large groups of reviewers\.

Amazon A2I makes it easy to build and manage human reviews for machine learning applications\. Amazon A2I provides built\-in human review workflows for common machine learning use cases, such as content moderation and text extraction from documents, which allows predictions from Amazon Rekognition and Amazon Textract to be reviewed easily\. You can also create your own workflows for ML models built on Amazon SageMaker or any other tools\. Using Amazon A2I, you can allow human reviewers to step in when a model is unable to make a high\-confidence prediction or to audit its predictions on an ongoing basis\.

![\[Amazon Augmented AI - How It Works\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/amazon-augmented-ai-how-it-works.png)

**Topics**
+ [Get Started with Amazon Augmented AI](a2i-getting-started.md)
+ [Use Task Types](a2i-task-types-general.md)
+ [Create a Flow Definition](a2i-create-flow-definition.md)
+ [Delete a Flow Definition](a2i-delete-flow-definition.md)
+ [Create and Start a Human Loop](a2i-start-human-loop.md)
+ [Create and Manage Worker Task Templates](a2i-instructions-overview.md)
+ [Monitor and Manage Your Human Loop](a2i-monitor-humanloop-results.md)
+ [Permissions and Security in Amazon Augmented AI](a2i-permissions-security.md)
+ [Use Amazon CloudWatch Events in Amazon Augmented AI](a2i-cloudwatch-events.md)
+ [Use APIs in Amazon Augmented AI](a2i-api-references.md)