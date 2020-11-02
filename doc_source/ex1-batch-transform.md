# Step 6\.2: Deploy the Model with Batch Transform<a name="ex1-batch-transform"></a>

To get inference for an entire dataset, use batch transform\. SageMaker stores the results in Amazon S3\.

For information about batch transforms, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\. For an example that uses batch transform, see the batch transform sample notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/sagemaker\_batch\_transform/introduction\_to\_batch\_transform](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_batch_transform/introduction_to_batch_transform)\.

**Topics**
+ [Deploy a Model with Batch Transform \(SageMaker High\-level Python Library\)](#ex1-batch-transform-api-high-level)
+ [Deploy a Model with Batch Transform \(SDK for Python \(Boto3\)\)](#ex1-batch-transform-api-low-level)

## Deploy a Model with Batch Transform \(SageMaker High\-level Python Library\)<a name="ex1-batch-transform-api-high-level"></a>

The following code creates a `sagemaker.transformer.Transformer` object from the model that you trained in [Create and Run a Training Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](ex1-train-model.md#ex1-train-model-sdk)\. Then it calls that object's `transform` method to create a transform job\. When you create the `sagemaker.transformer.Transformer` object, you specify the number and type of ML instances to use to perform the batch transform job, and the location in Amazon S3 where you want to store the inferences\. 

Paste the following code in a cell in the Jupyter notebook you created in [Step 3: Create a Jupyter Notebook](ex1-prepare.md) and run the cell\.

```
# The location of the test dataset
batch_input = 's3://{}/{}/test/examples'.format(bucket, prefix)

# The location to store the results of the batch transform job
batch_output = 's3://{}/{}/batch-inference'.format(bucket, prefix)

transformer = xgb_model.transformer(instance_count=1, instance_type='ml.m4.xlarge', output_path=batch_output)

transformer.transform(data=batch_input, data_type='S3Prefix', content_type='text/csv', split_type='Line')

transformer.wait()
```

 For more information, see [https://sagemaker\.readthedocs\.io/en/stable/transformer\.html](https://sagemaker.readthedocs.io/en/stable/transformer.html)\.

**Next Step**  
[Step 7: Validate the Model](ex1-test-model.md)

## Deploy a Model with Batch Transform \(SDK for Python \(Boto3\)\)<a name="ex1-batch-transform-api-low-level"></a>

To run a batch transform job, call the `create_transform_job`\. method using the model that you trained in [Deploy the Model to SageMaker Hosting Services \(AWS SDK for Python \(Boto3\)\)\.](ex1-deploy-model.md#ex1-deploy-model-boto)\.

**To create a batch transform job \(SDK for Python \(Boto3\)\)**

For each of the following steps, paste the code in a cell in the Jupyter notebook you created in [Step 3: Create a Jupyter Notebook](ex1-prepare.md) and run the cell\.

1. Name the batch transform job and specify where the input data \(the test dataset\) is stored and where to store the job's output\.

   ```
   batch_job_name = 'xgboost-mnist-batch' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   
   batch_input = 's3://{}/{}/test/examples'.format(bucket, prefix)
   print(batch_input)
   
   batch_output = 's3://{}/{}/batch-inference'.format(bucket, prefix)
   ```

1. Configure the parameters that you pass when you call the `create_transform_job` method\. 

   ```
   request = \
   {
       "TransformJobName": batch_job_name,
       "ModelName": model_name,
       "BatchStrategy": "MultiRecord",
       "TransformOutput": {
           "S3OutputPath": batch_output
       },
       "TransformInput": {
           "DataSource": {
               "S3DataSource": {
                   "S3DataType": "S3Prefix",
                   "S3Uri": batch_input 
               }
           },
           "ContentType": "text/csv",
           "SplitType": "Line",
           "CompressionType": "None"
       },
       "TransformResources": {
               "InstanceType": "ml.m4.xlarge",
               "InstanceCount": 1
       }
   }
   ```

   For more information about the parameters, see [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html)\.

1. Call the `create_transform_job` method, passing in the parameters that you configured in the previous step\. Then call the `describe_transform_job` method in a loop until it completes\.

   Paste the following code in a cell in the Jupyter notebook you created in [Step 3: Create a Jupyter Notebook](ex1-prepare.md) and run the cell\.

   ```
   sm.create_transform_job(**request)
                               
   while(True):
       response = sm.describe_transform_job(TransformJobName=batch_job_name)
       status = response['TransformJobStatus']
       if  status == 'Completed':
           print("Transform job ended with status: " + status)
           break
       if status == 'Failed':
           message = response['FailureReason']
           print('Transform failed with the following error: {}'.format(message))
           raise Exception('Transform job failed') 
       print("Transform job is still in status: " + status)    
       time.sleep(30)
   ```

**Next Step**  
[Step 7: Validate the Model](ex1-test-model.md)