# Specify a Default S3 Bucket to Store Training Output Data<a name="automatic-model-tuning-ex-bucket"></a>

Specify the name of the Amazon S3 bucket where you want to store the output of the training jobs\. The name of the bucket must contain **sagemaker**, and be globally unique\. The bucket must be in the same AWS Region as the notebook instance that you use for this example\.

**Note**  
The name of the bucket doesn't need to contain **sagemaker** if the role that you use to run the hyperparameter tuning job has a policy that gives the SageMaker service principle `S3FullAccess` permission\.

`prefix` is the path within the bucket where SageMaker stores the output from training jobs\.

```
sess = sagemaker.Session()
bucket = sess.default_bucket()                    # Set a default S3 bucket
prefix = 'sagemaker/DEMO-automatic-model-tuning-xgboost-dm'
```

## Next Step<a name="automatic-model-tuning-ex-next-data"></a>

[Download, Prepare, and Upload Training Data](automatic-model-tuning-ex-data.md)