# Step 4 : Invoke the Endpoint to Get Inferences<a name="mxnet-example-invoke"></a>

Your model is now deployed on Amazon SageMaker\. To get inferences, send requests using the `predict` method provided by the `MXNetPredictor` object \(returned in the preceding section\)\. The method calls the Amazon SageMaker [InvokeEndpoint](API_runtime_InvokeEndpoint.md)\. 

This example provides an HTML canvas that you can use to draw a number using your mouse\. The test code sends this image to the model for inference\.

1. Display the canvas by copying, pasting, and running the following code:

   ```
   from IPython.display import HTML
   HTML(open("/home/ec2-user/sample-notebooks/sagemaker-python-sdk/mxnet_mnist/input.html").read())
   ```

1. Use your mouse to raw a single\-digit number on the canvas\. 

1. Run the `predict` method to get inference from the model\. 

   ```
   response = predictor.predict(data)
   print('Raw prediction result:')
   print(response)
   
   labeled_predictions = list(zip(range(10), response[0]))
   print('Labeled predictions: ')
   print(labeled_predictions)
   
   labeled_predictions.sort(key=lambda label_and_prob: 1.0 - label_and_prob[1])
   print('Most likely answer: {}'.format(labeled_predictions[0]))
   ```

   The following is an example of output\. It shows the number that was inferred \(7\) and a number that represents the probability that the inference is correct\.

   ```
   Raw prediction result:
   [[1.7489463002839933e-11, 0.006231508683413267, 8.953022916102782e-05, 0.0872468426823616, 0.0001965702831512317, 1.7784617739380337e-05, 3.312719196180147e-11, 0.7383657097816467, 0.009811942465603352, 0.15804021060466766]]
   Labeled predictions: 
   [(0, 1.7489463002839933e-11), (1, 0.006231508683413267), (2, 8.953022916102782e-05), (3, 0.0872468426823616), (4, 0.0001965702831512317), (5, 1.7784617739380337e-05), (6, 3.312719196180147e-11), (7, 0.7383657097816467), (8, 0.009811942465603352), (9, 0.15804021060466766)]
   Most likely answer: (7, 0.7383657097816467)
   ```

In the result:

+ The **Raw prediction result** is a list of 10 probability values that the model returned as inference, corresponding to the digits 0 through 9\. From these values, the input digit is 7 based on the highest probability value \(0\.7383657097816467\)\.

+ The values are listed in order, one value for each digit \(0 through 9\)\. The model added labels to it and returned **Labeled predictions**\.

+ Based on the highest probability, our code returned the **Most likely answer** \(digit 7\)\. 

You can now delete the resources that you created in this exercise\. For more information, see [Step 4: Clean up](ex1-cleanup.md)\.

Your Amazon SageMaker notebook instance includes additional examples\.