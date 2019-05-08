# Using Automated Data Labeling<a name="sms-automated-labeling"></a>

Ground Truth can use *active learning* to automate the labeling of your input data\. Active learning is a machine learning technique that identifies data that should be labeled by your workers\.

Automated data labeling is optional\. Turn it on when you create a labeling job\. Automated data labeling incurs Amazon SageMaker training and inference costs, but it helps to reduce the cost and time that it takes to label your dataset compared to humans alone\.

Use automated data labeling on large datasets\. The neural networks used with active learning require a significant amount of data for every new dataset\. With larger datasets, there is more potential to automatically label the data and therefore reduce the total cost of labeling\. We recommend that you use thousands of data objects when using automated data labeling\. The system minimum for automated labeling is 1,250 objects, but to get a meaningful amount of your data automatically labeled, we strongly suggest a minimum with 5,000 or more objects\.

The potential benefit of automated data labeling also depends on the accuracy that you require\. Higher accuracy levels generally reduce the number of data objects that are automatically labeled\.

 

When Amazon SageMaker Ground Truth starts an automated data labeling job, it first selects a random sample of the input data\. Then it sends the sample to human workers\. When the labeled data are returned, Ground Truth uses this set of data as validation data\. It is used to validate the machine learning models that Ground Truth trains for automated data labeling\.

Next, Ground Truth runs an Amazon SageMaker batch transform using the validation set\. This generates a quality metric that Ground Truth uses to estimate the potential quality of auto\-labeling the rest of the unlabeled data\.

Ground Truth next runs an Amazon SageMaker batch transform on the unlabeled data in the dataset\. Any data where the expected quality of automatically labeling the data is above the requested level of accuracy is considered labeled\.

After performing the auto\-labeling step, Ground Truth selects a new sample of the most ambiguous unlabeled data points in the dataset\. It sends those to human workers\. Ground Truth uses the existing labeled data and this additional labeled data from human workers to train a new model\. The process is repeated until the dataset is fully labeled\.

## Amazon EC2 Instances Required for Automated Data Labeling<a name="sms-auto-labeling-ec2"></a>

To run automated data labeling, Ground Truth requires the following Amazon EC2 resources for training and batch inference jobs:


| Automated labeling action | Training instance type | Inference instance type | 
| --- | --- | --- | 
| Image classification | ml\.p3\.2xlarge | ml\.c5\.xlarge | 
| Object detection | ml\.p3\.2xlarge | ml\.c5\.4large | 
| Text classification | ml\.c5\.2xlarge | ml\.m4\.xlarge | 

To check the Amazon EC2 service limits for your account, and to request additional resources if necessary, see [ Amazon EC2 Service Limits ](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html)\.