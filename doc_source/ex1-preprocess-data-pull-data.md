# Step 4\.1: Download the MNIST Dataset<a name="ex1-preprocess-data-pull-data"></a>

To download the MNIST dataset, copy and paste the following code into the notebook and run it:

```
import boto3
import pickle
​
# download preprocessed data from S3
s3 = boto3.client('s3')
​
bucket = 'sagemaker-sample-files'
s3.download_file(bucket, 'datasets/image/MNIST/mnist.pkl', './mnist.pkl')
​
with open('mnist.pkl', 'rb') as f:
    train_set, valid_set, test_set = pickle.load(f)
```

The code does the following:

1. Downloads the MNIST dataset \(`mnist.pkl`\) from the SageMaker public database S3 bucket to your notebook instance\.

1. Unzips the file and reads the following datasets into the notebook instance's memory:
   + `train_set` – You use these images of handwritten numbers to train a model\.
   + `valid_set` – The [XGBoost Algorithm](xgboost.md) uses these images to evaluate the progress of the model during training\.
   + `test_set` – You use this set to get inferences to test the deployed model\.

**Next Step**  
[Step 4\.2: Explore the Training Dataset](ex1-preprocess-data-inspect.md)