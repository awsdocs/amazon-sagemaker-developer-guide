# Specify a S3 Bucket to Upload Training Datasets and Store Output Data<a name="automatic-model-tuning-ex-bucket"></a>

Set up a S3 bucket to upload training datasets and save training output data\.

**To use a default S3 bucket**

Use the following code to specify the default S3 bucket allocated for your SageMaker session\. `prefix` is the path within the bucket where SageMaker stores the data for the current training job\.

```
sess = sagemaker.Session()
bucket = sess.default_bucket()                    # Set a default S3 bucket
prefix = 'DEMO-automatic-model-tuning-xgboost-dm'
```

**\(Optional\) To use a specific S3 bucket**

If you want to use a specific S3 bucket, use the following code and replace the strings to the exact name of the S3 bucket\. The name of the bucket must contain **sagemaker**, and be globally unique\. The bucket must be in the same AWS Region as the notebook instance that you use for this example\.

```
bucket = "sagemaker-your-preferred-s3-bucket"
```

**Note**  
The name of the bucket doesn't need to contain **sagemaker** if the IAM role that you use to run the hyperparameter tuning job has a policy that gives the `S3FullAccess` permission\.

## Next Step<a name="automatic-model-tuning-ex-next-data"></a>

[Download, Prepare, and Upload Training Data](automatic-model-tuning-ex-data.md)