# Get Inferences for an Entire Dataset with Batch Transform<a name="how-it-works-batch"></a>

 Get inferences for an entire dataset by using Amazon SageMaker batch transform\. Batch transform uses a trained model to get inferences on a dataset that is stored in Amazon S3, and saves the inferences in an S3 bucket that you specify when you create a batch transform job\. Batch transform manages all compute resources necessary to get inferences\. This includes launching instances and deleting them after the transform job completes\. 

To perform a batch transform, create a transform job, which includes the following information:
+ The path to the S3 bucket where you've stored the data to transform\.
+ The compute resources that you want Amazon SageMaker to use for the transform job\. *Compute resources* are ML compute instances that are managed by Amazon SageMaker\.
+ The path to the S3 bucket where you want to store the output of the job\.
+ The name of the model that you want to use in the transform job\.

For examples on how to use batch transform, see [Step 2\.4\.2: Deploy the Model with Batch Transform ](ex1-batch-transform.md)\.

Batch transform is ideal for situations where:
+ You want to get inferences for an entire dataset and store them online\.
+ You don't need a persistent endpoint that applications \(for example, web or mobile apps\) can call to get inferences\.
+  You don't need the sub\-second latency that Amazon SageMaker hosted endpoints provide\. 
+ You want to preprocess your data before using the data to train a new model or generate inferences\.

The following diagram illustrates the workflow of a batch transform job:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/batch-transform.png)

## Best Practices for Using Batch Transform<a name="considerations-batch"></a>
+ You can create a transform job by using the Amazon SageMaker console or the API\. For more information, see [CreateTransformJob](API_CreateTransformJob.md) API\.
+ After you create a transform job, Amazon SageMaker launches the ML compute instances and uses the model you specify to transform the input data\. It stores the results and other output in the S3 bucket that you specified\.
+ Amazon SageMaker uses [Multipart Upload API](https://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html) to upload output data results from a transform job to S3\. Typically all multipart uploads run to completion\. In case of an error, the multipart upload is stopped and then removed from S3\. However there are cases such as a network outage where a multipart upload remains incomplete in S3\. To avoid extra storage charges in these cases, we recommend that you add the [S3 bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html#mpu-abort-incomplete-mpu-lifecycle-config) to remove incomplete multipart uploads that are stored in the output S3 buckets\.
+ For testing model variants, create separate transform jobs for each variant using a validation data set\. You can then analyze the results using metrics\. Make sure you specify a different `ModelName` and a unique `S3OutputPath` location for each transform job\.
+ For large datasets or data of indeterminate size, such as a video stream, you can create an *infinite stream*\. An infinite stream is when a transform Job is set to continuously input and transform data\. The transform job completes either when all the data is transformed or when it is instructed to stop\. This approach works only with models from supported algorithms\. To create an infinite stream using the API, in the `CreateTransformJob` request: set `SplitType` to `None` and set `MaxPayloadInMB` to 0\. To create an infinite stream using the console, choose **None** for **Split type** and set **Max payload size \(MB\)** to 0\. Amazon SageMaker interprets a `MaxPayloadInMB` of 0 as no limit on the payload size\.

## How It Works: Next Topic<a name="how-it-works-batch-next-topic"></a>

[Validate a Machine Learning Model](how-it-works-model-validation.md)