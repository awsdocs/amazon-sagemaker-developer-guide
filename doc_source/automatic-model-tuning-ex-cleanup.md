# Clean up<a name="automatic-model-tuning-ex-cleanup"></a>

To avoid incurring unnecessary charges, when you are done with the example, use the AWS Management Console to delete the resources that you created for it\. 

**Note**  
If you plan to explore other examples, you might want to keep some of these resources, such as your notebook instance, S3 bucket, and IAM role\.

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and delete the notebook instance\. Stop the instance before deleting it\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/) and delete the bucket that you created to store model artifacts and the training dataset\. 

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/) and delete the IAM role\. If you created permission policies, you can delete them, too\.

1. Open the Amazon CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/) and delete all of the log groups that have names starting with `/aws/sagemaker/`\.