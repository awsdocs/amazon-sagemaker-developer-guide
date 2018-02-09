# Step 3\.5: Validate the Model<a name="ex1-test-model"></a>

You now have a model that is deployed in Amazon SageMaker\. To validate the model, send sample requests and get inferences\. To send requests to an Amazon SageMaker endpoint, use the [InvokeEndpoint](API_runtime_InvokeEndpoint.md) API\. 

To validate your model, choose one of the following options\. 

+ **Use the high\-level Python library provided by Amazon SageMaker**\. 

  The `kmeans_predictor` returned by the `deploy` call in the preceding step provides the `predict` method\. To get inferences from the model, call this method\. 

  1. Get an inference for the 30th image of a handwritten number in the `valid_set` dataset\.

     ```
     result = kmeans_predictor.predict(train_set[0][30:31])
     print(result)
     ```

     Example response: 

     ```
     [label {
       key: "closest_cluster"
       value {
         float32_tensor {
           values: 3.0
         }
       }
     }
     label {
       key: "distance_to_cluster"
       value {
         float32_tensor {
           values: 7.221197605133057
         }
       }
     }
     ]
     ```

     The response shows that the input image belongs to cluster 3\. It also shows the mean squared distance for that cluster\. 
**Note**  
In the k\-means implementation, the cluster numbers and digit they represent don't align\. For example, the algorithm might group images of the handwritten number 3 in cluster 0, and images of the number 4 in cluster 9\.

  1. Get inferences for the first 100 images\.

     ```
     %%time 
     
     result = kmeans_predictor.predict(valid_set[0][0:100])
     clusters = [r.label['closest_cluster'].float32_tensor.values[0] for r in result]
     ```

     Visualize the results\.

     ```
     for cluster in range(10):
         print('\n\n\nCluster {}:'.format(int(cluster)))
         digits = [ img for l, img in zip(clusters, valid_set[0]) if int(l) == cluster ]
         height=((len(digits)-1)//5)+1
         width=5
         plt.rcParams["figure.figsize"] = (width,height)
         _, subplots = plt.subplots(height, width)
         subplots=numpy.ndarray.flatten(subplots)
         for subplot, image in zip(subplots, digits):
             show_digit(image, subplot=subplot)
         for subplot in subplots[len(digits):]:
             subplot.axis('off')
     
         plt.show()
     ```

     This code takes the first100 images of handwritten numbers from the `valid_set` dataset and generates inferences for them\. The result is a set of clusters that group similar images\. The following visualization shows four of the clusters that the model returned:   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ironman-validate-kmeans-model-10.png)

+ **Use the SDK for Python**\. 

  To send requests to the endpoint, use the `invoke_endpoint` method\. 

  1. Send a request that sends the 30th image in the `train_set` as input\. Each image is a 28x28 \(total of 784\) pixel image\. The request sends all 784 pixels in the image as comma\-separated values\. 

     ```
     import json
     
     # Simple function to create a csv from our numpy array
     def np2csv(arr):
         csv = io.BytesIO()
         numpy.savetxt(csv, arr, delimiter=',', fmt='%g')
         return csv.getvalue().decode().rstrip()
     
     runtime = boto3.Session().client('sagemaker-runtime')
     
     payload = np2csv(train_set[0][30:31])
     
     response = runtime.invoke_endpoint(EndpointName=endpoint_name, 
                                        ContentType='text/csv', 
                                        Body=payload)
     result = json.loads(response['Body'].read().decode())
     print(result)
     ```

     In the following example response, the inference classifies the image as belonging to cluster 7 \(`labels` identifies the cluster\)\. The inference also shows the mean squared distance for that cluster\.

     ```
     {'predictions': [{'distance_to_cluster': 7.2033820152282715, 'closest_cluster': 7.0}]}
     ```

  1. Run another test\. The following code takes the first 100 images from the `valid_set` validation set and generates inferences for them\. This test identifies the cluster that the input images belong to *and* provides a visual representation of the result\.

     ```
     %%time 
     
     payload = np2csv(valid_set[0][0:100])
     response = runtime.invoke_endpoint(EndpointName=endpoint_name, 
                                        ContentType='text/csv', 
                                        Body=payload)
     result = json.loads(response['Body'].read().decode())
     clusters = [p['closest_cluster'] for p in result['predictions']]
     
     for cluster in range(10):
         print('\n\n\nCluster {}:'.format(int(cluster)))
         digits = [ img for l, img in zip(clusters, valid_set[0]) if int(l) == cluster ]
         height=((len(digits)-1)//5)+1
         width=5
         plt.rcParams["figure.figsize"] = (width,height)
         _, subplots = plt.subplots(height, width)
         subplots=numpy.ndarray.flatten(subplots)
         for subplot, image in zip(subplots, digits):
             show_digit(image, subplot=subplot)
         for subplot in subplots[len(digits):]:
             subplot.axis('off')
     
         plt.show()
     ```

     The result is a set of clusters that group similar images\. The following visualization shows four of the clusters that the model returned:   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ironman-validate-kmeans-model-10.png)

  1. To get an idea of how accurate the model is, review the clusters and the numbers in them to see how well the model clustered similar looking digits\. To improve the model, you might make the following changes to the training job:

     + Change the model training parameters—For example, increase the number of epochs or tweak hyperparameters, such `extra_center_factor`\. For more information, see [K\-Means Hyperparameters](k-means-api-config.md)\.

     + Consider switching the algorithm—The images in the MNIST dataset include information that identifies the digits, called labels\. Similarly, you might be able to label your training data for other problems\. You might then use the label information and a supervised algorithm, such as the linear learner algorithm provided by Amazon SageMaker\. For more information, see [Linear Learner](linear-learner.md)\.

     + Try a more specialized algorithm—Try a specialized algorithm, such as the image classification algorithm provided by Amazon SageMaker instead of the linear learner algorithm\. For more information, see [Image Classification Algorithm](image-classification.md)\. 

     + Use a custom algorithm—Consider using a custom neural network algorithm built on Apache MXNet or TensorFlow\. For more information, see [Using Apache MXNet with Amazon SageMaker](mxnet.md) and [Using TensorFlow with Amazon SageMaker](tf.md)\.

**Next Step**  
[Step 4: Clean up](ex1-cleanup.md)