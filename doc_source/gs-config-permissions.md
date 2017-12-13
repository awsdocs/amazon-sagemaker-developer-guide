# Step 1\.2: Create an S3 Bucket<a name="gs-config-permissions"></a>

In exercises where you create a model training job, you save the following in an Amazon S3 bucket:

+ The model training data

+ Model artifacts, which Amazon SageMaker generates during model training 

You can store the training data and artifacts in a single bucket or in two separate buckets\. For exercises in this guide, one bucket is sufficient\. You can use existing buckets or create new ones\. 

Follow the instructions in [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. Include `sagemaker` in the bucket name; for example, `sagemaker-``datetime`\. 

**Note**  
Amazon SageMaker needs your permissions to access this bucket\. You grant these permissions with an IAM role, which you create in the next step \(as part of creating an Amazon SageMaker notebook instance\)\. This IAM role automatically gets permissions to access any bucket with `sagemaker` in the name through the `AmazonSageMakerFullAccess` policy that Amazon SageMaker attaches to the role\. 

## Next Step<a name="gs-setup-ws-nextstep"></a>

[Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)