# Step 1: Create an Amazon S3 Bucket<a name="gs-config-permissions"></a>

Training a model produces the following
+ The model training data
+ Model artifacts, which Amazon SageMaker generates during model training 

You save these in an Amazon Simple Storage Service \(Amazon S3\) bucket: You can store datasets that you use as your training data and model artifacts that are the output of a training job in a single bucket or in two separate buckets\. For this exercise and others in this guide, one bucket is sufficient\. If you already have S3 buckets, you can use them, or you can create new ones\. 

To create a bucket, follow the instructions in [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. Include `sagemaker` in the bucket name\. For example, `sagemaker-``datetime`\.

**Note**  
Amazon SageMaker needs permission to access these buckets\. You grant permission with an IAM role, which you create in the next step when you create a SageMaker notebook instance\. This IAM role automatically gets permissions to access any bucket that has `sagemaker` in the name\. It gets these permissions through the `AmazonSageMakerFullAccess` policy, which SageMaker attaches to the role\. If you add a policy to the role that grants the SageMaker service principal `S3FullAccess` permission, the name of the bucket does not need to contain **sagemaker**\.

## Next Step<a name="gs-setup-ws-nextstep"></a>

[Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)