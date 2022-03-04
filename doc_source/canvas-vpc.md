# Run Amazon SageMaker Canvas in a VPC<a name="canvas-vpc"></a>

You can run Amazon SageMaker Canvas in a Amazon Virtual Private Cloud that you've created\. SageMaker Canvas can interact with other AWS services using either an internet connection or a VPC endpoint\. When your users build models on their data, SageMaker Canvas creates machine learning training jobs that run outside of your VPC\.

SageMaker Canvas only accesses other AWS services to manage and store data for its functionality\. For example, it connects to Amazon Redshift if your users access a Amazon Redshift database\. It can connect to an AWS service such as Amazon Redshift using an internet connection or a VPC endpoint\.

Use a VPC endpoint if you want a private connection to an AWS service that uses a networking path that is isolated from the public internet\. For example, if you access Amazon S3 using a VPC endpoint, the SageMaker Canvas application that you're using within the VPC accesses Amazon S3 through the endpoint\. The communication between SageMaker Canvas and Amazon S3 is private\. You can specify endpoints for the following services:
+ Amazon Athena
+ Amazon SageMaker
+ AWS Security Token Service
+ Amazon ECR
+ Amazon S3
+ Amazon Redshift
+ AWS Secrets Manager
+ Amazon CloudWatch
+ Amazon CloudWatch Logs

The following are VPC endpoints for the preceding services:
+ com\.amazonaws\.*Region*\.athena
+ com\.amazonaws\.*Region*\.sagemaker\.api
+ com\.amazonaws\.*Region*\.sts
+ com\.amazonaws\.*Region*\.sagemaker\.runtime
+ com\.amazonaws\.*Region*\.ecr\.api
+ com\.amazonaws\.*Region*\.ssm
+ com\.amazonaws\.*Region*\.ecr\.dkr
+ com\.amazonaws\.*Region*\.logs
+ com\.amazonaws\.*Region*\.s3
+ com\.amazonaws\.*Region*\.monitoring
+ aws\.sagemaker\.*Region*\.notebook
+ redshift\-data\.*Region*\.amazonaws\.com
+ secretsmanager\.*Region*\.amazonaws\.com

You only need to use VPC endpoints if your VPC doesn't have access to the internet\. If your VPC doesn't have access to the internet, you only need to specify endpoints to Amazon Redshift and Amazon Athena if your users are accessing those services\.

For information about setting up a VPC in Amazon SageMaker, see [Choose a VPC](onboard-vpc.md)\.