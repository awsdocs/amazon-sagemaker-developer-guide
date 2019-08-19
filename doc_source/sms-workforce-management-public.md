# Using the Amazon Mechanical Turk Workforce<a name="sms-workforce-management-public"></a>

The Amazon Mechanical Turk workforce provides the most workers for your labeling job\.

You can use the console to choose the Amazon Mechanical Turk workforce for your labeling job, or you can provide the Amazon Resource Name \(ARN\) for the Amazon Mechanical Turk workforce when you use the [CreateLabelingJob](API_CreateLabelingJob.md) operation\.

Any Amazon Mechanical Turk workforce billing is handled as part of your Amazon SageMaker Ground Truth billing\. You do not need to create a separate Mechanical Turk account to use the Amazon Mechanical Turk workforce\.

 The ARN for the Amazon Mechanical Turk workforce is: 
+ ` arn:aws:sagemaker:region:394669845002:workteam/public-crowd/default`

The Amazon Mechanical Turk workforce is a world\-wide resource\. Workers are available 24 hours a day, 7 days a week\. You typically get the fastest turn\-around for your labeling jobs when you use the Amazon Mechanical Turk workforce\.

Adjust the number of workers that annotate each data object based on the complexity of the job and the quality that you need\. Ground Truth uses annotation consolidation to improve the quality of the labels\. More workers can make a difference in the quality of the labels for more complex labeling jobs, but might not make a difference for simpler jobs\. For more information, see [Annotation Consolidation](sms-annotation-consolidation.md)\.

**Note**  
Only use the Amazon Mechanical Turk workforce to label data that is public or has been stripped of any sensitive information\. You should not use the Amazon Mechanical Turk workforce for a dataset that contains personally identifiable information, such as names or email addresses\.

To choose the Amazon Mechanical Turk workforce when you are creating a labeling job using the console, do the following during the **Select workers and configure tool** step:

**To use the Amazon Mechanical Turk workforce**

1. Choose **Public** from **Worker types**\.

1. Choose **The dataset does not contain adult content** if your dataset doesn't contain potentially offensive content\. This enables workers to opt out if they don't want to work with it\.

1. Acknowledge that your data will be viewed by the Amazon Mechanical Turk workforce and that all personally identifiable information \(PII\) has been removed\.

1. Choose **Additional configuration** to set optional parameters\.

1. Optional\. Enable automated data labeling to have Ground Truth automatically label some of your dataset\. For more information, see [Using Automated Data Labeling](sms-automated-labeling.md)\.

1. Optional\. Set the number of workers that should see each object in your dataset\. Using more workers can increase the quality of your labels but also increases the cost\.

Your labeling job will now be sent to the Amazon Mechanical Turk workforce\. You can use the console to continue configuring your labeling job\.