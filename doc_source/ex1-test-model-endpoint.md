# Step 7\.1: Validate a Model Deployed to SageMaker Hosting Services<a name="ex1-test-model-endpoint"></a>

If you deployed a model to Amazon SageMaker hosting services in [Step 6\.1: Deploy the Model to SageMaker Hosting Services](ex1-deploy-model.md), you now have an endpoint that you can invoke to get inferences in real time\. To validate the model, invoke the endpoint with example images from the test dataset and check whether the inferences you get match the actual labels of the images\.

**Topics**
+ [Validate a Model Deployed to SageMaker Hosting Services \(SageMaker Python SDK\)](#ex1-test-model-endpoint-sdk)
+ [Validate a Model Deployed to SageMaker Hosting Services \(AWS SDK for Python \(Boto3\)\)](#ex1-test-model-endpoint-boto)

## Validate a Model Deployed to SageMaker Hosting Services \(SageMaker Python SDK\)<a name="ex1-test-model-endpoint-sdk"></a>

To validate the model by using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), use the `sagemaker.predictor.RealTimePredictor` object that you created in [Deploy the Model to SageMaker Hosting Services \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](ex1-deploy-model.md#ex1-deploy-model-sdk)\. For information, see [https://sagemaker\.readthedocs\.io/en/stable/predictors\.html\#sagemaker\.predictor\.RealTimePredictor](https://sagemaker.readthedocs.io/en/stable/predictors.html#sagemaker.predictor.RealTimePredictor)\.

**To validate the model \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)**

1. Download the test data from Amazon S3\.

   ```
   s3 = boto3.resource('s3')
   
   test_key = "{}/test/examples".format(prefix)
   
   s3.Bucket(bucket).download_file(test_key, 'test_data')
   ```

1. Plot the first 10 images from the test dataset with their labels\.

   ```
   %matplotlib inline
                           
   for i in range (0, 10):
       img = test_set[0][i]
       label = test_set[1][i]
       img_reshape = img.reshape((28,28))
       imgplot = plt.imshow(img_reshape, cmap='gray')
       print('This is a {}'.format(label))
       plt.show()
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/test-digits.png)

1. To get inferences for the first 10 examples in the test dataset, call the `predict` method of the `sagemaker.predictor.RealTimePredictor` object\.

   ```
   with open('test_data', 'r') as f:
       for j in range(0,10):
           single_test = f.readline()
           result = xgb_predictor.predict(single_test)
           print(result)
   ```

   To see if the model is making accurate predictions, check the output from this step against the numbers that you plotted in the previous step\.

You have now trained, deployed, and validated your first model in SageMaker\.

**Next Step**  
[Step 9: Clean Up](ex1-cleanup.md)

## Validate a Model Deployed to SageMaker Hosting Services \(AWS SDK for Python \(Boto3\)\)<a name="ex1-test-model-endpoint-boto"></a>

To use the AWS SDK for Python \(Boto3\) to validate the model, call the `invoke_endpoint` method\. This method corresponds to the [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) API provided by the SageMaker runtime\.

**To validate the model \(AWS SDK for Python \(Boto3\)\)**

1. Download the test data from Amazon S3\.

   ```
   s3 = boto3.resource('s3')
   
   test_key = "{}/test/examples".format(prefix)
   
   s3.Bucket(bucket).download_file(test_key, 'test_data')
   ```

1. Plot the first 10 images from the test dataset with their labels\.

   ```
   %matplotlib inline
                           
   for i in range (0, 10):
       img = test_set[0][i]
       label = test_set[1][i]
       img_reshape = img.reshape((28,28))
       imgplot = plt.imshow(img_reshape, cmap='gray')
       print('This is a {}'.format(label))
       plt.show()
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/test-digits.png)

1. Get the SageMaker runtime client, which provides the `invoke_endpoint` method\.

   ```
   runtime_client = boto3.client('runtime.sagemaker')
   ```

1. Get inferences from the first 10 examples in the test dataset by calling `invoke_endpoint`\.

   ```
   with open('test_data', 'r') as f:
       
       for i in range(0,10):
           single_test = f.readline()
           response = runtime_client.invoke_endpoint(EndpointName = endpoint_name,
                                            ContentType = 'text/csv',
                                            Body = single_test)
           result = response['Body'].read().decode('ascii')
           print('Predicted label is {}.'.format(result))
   ```

1. To see if the model is making accurate predictions, check the output from this step against the numbers you plotted in the previous step\.

You have now trained, deployed, and validated your first model in SageMaker\.

**Next Step**  
[Step 9: Clean Up](ex1-cleanup.md)