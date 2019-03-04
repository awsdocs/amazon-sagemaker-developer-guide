# Step 3: Create an Amazon S3 Bucket<a name="gs-config-permissions"></a>

In this section, you create an Amazon S3 bucket\. You use this bucket to store training data and the results of model training, called model artifacts\.
+ The model training data
+ Model artifacts, which Amazon SageMaker generates during model training 

You can store the training data and artifacts in a single bucket or in two separate buckets\. For exercises in this guide, one bucket is sufficient\. You can use existing buckets or create new ones\. 

Follow the instructions in [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. Include `sagemaker` in the bucket name; for example, `sagemaker-``datetime`\. 

**Note**  
Amazon SageMaker needs permission to access this bucket\. You grant permission with an IAM role, which you create in the next step \(as part of creating an Amazon SageMaker notebook instance\)\. This IAM role automatically gets permissions to access any bucket with `sagemaker` in the name through the `AmazonSageMakerFullAccess` policy that Amazon SageMaker attaches to the role\. 

Now you are ready to begin the [Step 1: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md) topic that walks you through procedures for creating a notebook, training and deploying a machine learning Amazon SageMaker model\.