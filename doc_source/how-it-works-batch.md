# Get Inferences for an Entire Dataset with Batch Transform<a name="how-it-works-batch"></a>

To get inferences for an entire dataset, use batch transform\. With batch transform, you create a batch transform job using a trained model and the dataset, which must be stored in Amazon S3\. Amazon SageMaker saves the inferences in an S3 bucket that you specify when you create the batch transform job\. Batch transform manages all of the compute resources required to get inferences\. This includes launching instances and deleting them after the batch transform job has completed\. Batch transform manages interactions between the data and the model with an object within the instance node called an agent\.

Use batch transform when you:
+ Want to get inferences for an entire dataset and index them to serve inferences in real time
+ Don't need a persistent endpoint that applications \(for example, web or mobile apps\) can call to get inferences
+ Don't need the subsecond latency that Amazon SageMaker hosted endpoints provide

You can also use batch transform to preprocess your data before using it to train a new model or generate inferences\.

The following diagram shows the workflow of a batch transform job:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/batch-transform-v2.png)

To perform a batch transform, create a batch transform job using either the Amazon SageMaker console or the API\. Provide the following:
+ The path to the S3 bucket where you've stored the data that you want to transform\.
+ The compute resources that you want Amazon SageMaker to use for the transform job\. *Compute resources* are machine learning \(ML\) compute instances that are managed by Amazon SageMaker\.
+ The path to the S3 bucket where you want to store the output of the job\.
+ The name of the Amazon SageMaker model that you want to use to create inferences\. You must use a model that you have already created either with the [CreateModel](API_CreateModel.md) operation or the console\.

For example, if your dataset has two files, `input1.csv` and `input2.csv` stored in an S3 bucket, your input file might look as follows\. 

```
An example of input file input1.csv content:
                Record1-Attribute1, Record1-Attribute2, Record1-Attribute3, ..., Record1-AttributeM
                Record2-Attribute1, Record2-Attribute2, Record2-Attribute3, ..., Record2-AttributeM
                Record3-Attribute1, Record3-Attribute2, Record3-Attribute3, ..., Record3-AttributeM
                ...
                RecordN-Attribute1, RecordN-Attribute2, RecordN-Attribute3, ..., RecordN-AttributeM
```

For an example of how to use batch transform, see [Step 2\.4\.2: Deploy the Model with Batch Transform ](ex1-batch-transform.md)\.

## Use Batch Transform with Large Datasets<a name="large-datasets-batch"></a>

When a batch transform job starts, Amazon SageMaker initializes the compute instances and distributes the input data workload between them\. For example, one instance might process `input1.csv`, and the other instance might process `input2.csv`\. To keep large payloads within the [MaxPayloadInMB](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB) limit, which you set when you create the batch transform job, you might split an input file into several fractions or mini\-batches\.

```
An example of a mini-batch created from input1.csv:
                Record3-Attribute1, Record3-Attribute2, Record3-Attribute3, ..., Record3-AttributeM
                Record4-Attribute1, Record4-Attribute2, Record4-Attribute3, ..., Record4-AttributeM
```

Split input files into mini\-batches by setting the [SplitType](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TransformInput.html#SageMaker-Type-TransformInput-SplitType) hyperparameter as `Line`\. 

If the batch transform job successfully processes all of the records in an input file, it creates an output file with the same name and the `.out` suffix\. For multiple input files, such as `input1.csv` and `input2.csv`, the output files are named `input1.csv.out`, and `input2.csv.out`\. The batch transform job stores the output files at the specified location in Amazon S3, such as `s3://awsexamplebucket/output/`\. The predictions in an output file are listed in the same order as the records in the input file\.

```
An example of output file input1.csv.out content:
                Inference1-Attribute1, Inference1-Attribute2, Inference1-Attribute3, ..., Inference1-AttributeM
                Inference2-Attribute1, Inference2-Attribute2, Inference2-Attribute3, ..., Inference2-AttributeM
                Inference3-Attribute1, Inference3-Attribute2, Inference3-Attribute3, ..., Inference3-AttributeM
                ...
                InferenceN-Attribute1, Inference3-Attribute2, Inference3-Attribute3, ..., InferenceN-AttributeM
```

To combine the results, set the [AssembleWith](https://docs.aws.amazon.com/sagemaker/latest/dg/API_TransformOutput.html#SageMaker-Type-TransformOutput-AssembleWith) hyperparameter to `Line`\.

For more information about using the API to create a batch transform job, see the [CreateTransformJob](API_CreateTransformJob.md) API\. For more information about the correlation between batch transform input and output objects, see [OutputDataConfig](https://docs.aws.amazon.com/sagemaker/latest/dg/API_OutputDataConfig.html)\. For examples of how to use batch transform, see [Step 2\.4\.2: Deploy the Model with Batch Transform ](ex1-batch-transform.md)\.

## Use Batch Transform to Preprocess Data<a name="preprocessing-batch"></a>

To speed up preprocessing your dataset, you can use batch transform to preprocess a subset of your data, train a regression\-type model, and transform the rest of your data\. Batch transform might speed preprocessing when you have many time\-consuming preprocessing steps or you are working with large datasets\.

**To use batch transform to preprocess data**

1. Select several records from your dataset\. We suggest using records that represent edge cases with respect to the variance in your data\. You will use these records to train a model\.

1. Preprocess your selected records and save them as a separate version\.

1. Train a model using both the raw and preprocessed forms of the records\. This trains the model predict the preprocessed data\. For more information, see [Train a Model with Amazon SageMaker ](how-it-works-training.md)\.

1. Use the model and the entire dataset to generate inferences\. The results is the preprocessed data\.

## Speed Up a Batch Transform Job<a name="reduce-batch-transform-time"></a>

To reduce the time it takes to complete batch transform jobs using the API, use tunable hyperparameters, such as [MaxPayloadInMB](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB), [MaxConcurrentTransforms](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxConcurrentTransforms), and [BatchStrategy](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-BatchStrategy)\. Amazon SageMaker automatically finds the optimal hyperparameter settings for built\-in algorithms\. For custom algorithms, you need to provide these values through an [execution\-parameters](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-batch-code.html#your-algorithms-batch-code-how-containe-serves-requests) endpoint\.

To reduce the time it takes to complete batch transform jobs using the console, use tunable hyperparameters, such as **Max payload size \(MB\)**, **Max concurrent transforms**, and **Batch strategy** in the **Additional configuration** section of the **Batch transform job configuration** page\. Amazon SageMaker automatically finds the optimal hyperparameter settings for built\-in algorithms\. For custom algorithms, you need to provide these values through an [execution\-parameters](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-batch-code.html#your-algorithms-batch-code-how-containe-serves-requests) endpoint\.

## Use Batch Transform to Test Production Variants<a name="batch-transform-variant-testing"></a>

To test model variants, create a separate transform job for each variant and use a validation dataset\. For each transform job, specify a unique model name and location in Amazon S3 for the output file\. To analyze the results, use [Use Logs and Metrics to Monitor an Inference Pipeline](inference-pipeline-logs-metrics.md)\.

## Batch Transform Errors<a name="considerations-batch"></a>

Amazon SageMaker uses the Amazon S3 [Multipart Upload API](https://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html) to upload results from a batch transform job to Amazon S3\. If an error occurs, the uploaded results are removed from Amazon S3\. In some cases, such as a network outage, an incomplete multipart upload might remain in Amazon S3\. To avoid incurring storage charges, we recommend that you add the [S3 bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html#mpu-abort-incomplete-mpu-lifecycle-config) to the S3 bucket lifecycle rules\. This policy deletes incomplete multipart uploads that might be stored in the S3 bucket\. For more information, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html)

If a batch transform job fails to process an input file because there is a problem with the dataset, Amazon SageMaker marks the job as "failed" to alert you\. If an input file contains a bad record, the transform job doesn't create an output file for that input file because it can't maintain the same order in the transformed data\. When your dataset has multiple input files, a transform job continues to process input files even if it fails to process one\. The processed files still generate useable results\.

If you are using your own algorithms, you can use placeholder text, such as `ERROR`, when the algorithm finds a bad record in an input file\. For example, if the last record in a dataset is bad, the algorithm should place the error placeholder for the last record in the output file\.

## How It Works: Next Topic<a name="how-it-works-batch-next-topic"></a>

[Validate a Machine Learning Model](how-it-works-model-validation.md)