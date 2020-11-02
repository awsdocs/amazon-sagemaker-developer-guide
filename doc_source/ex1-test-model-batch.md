# Step 7\.2: Validate a Model Deployed with Batch Transform<a name="ex1-test-model-batch"></a>

You now have a file in Amazon S3 that contains inferences that you got by running a batch transform job in [Step 6\.2: Deploy the Model with Batch Transform](ex1-batch-transform.md)\. To validate the model, check a subset of the inferences from the file to see whether they match the actual numbers from the test dataset\.

**To validate the batch transform inferences**

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

1. Download the output from the batch transform job from Amazon S3 to a local file\.

   ```
   s3.Bucket(bucket).download_file(prefix + '/batch-inference/examples.out',  'batch_results')
   ```

1. Get the first 10 results from the batch transform job\.

   ```
   with open('batch_results') as f:
       results = f.readlines()
   for j in range (0, 10):
       print(results[j])
   ```

1. To see if the batch transform job made accurate predictions, check the output from this step against the numbers that you plotted from the test data\.

You have now trained, deployed, and validated your first model in SageMaker\.

**Next Step**  
[Step 8: Integrating Amazon SageMaker Endpoints into Internet\-facing Applications](getting-started-client-app.md)