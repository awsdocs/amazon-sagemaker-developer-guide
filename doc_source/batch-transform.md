# Use Batch Transform<a name="batch-transform"></a>

Use batch transform when you need to do the following: 
+ Preprocess datasets to remove noise or bias that interferes with training or inference from your dataset\.
+ Get inferences from large datasets\.
+ Run inference when you don't need a persistent endpoint\.
+ Associate input records with inferences to assist the interpretation of results\.

To filter input data before performing inferences or to associate input records with inferences about those records, see [Associate Prediction Results with Input Records](batch-transform-data-processing.md)\. For example, you can filter input data to provide context for creating and interpreting reports about the output data\.

For more information about batch transforms, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\.

**Topics**
+ [Use Batch Transform to Get Inferences from Large Datasets](#batch-transform-large-datasets)
+ [Speed up a Batch Transform Job](#batch-transform-reduce-time)
+ [Use Batch Transform to Test Production Variants](#batch-transform-test-variants)
+ [Batch Transform Errors](#batch-transform-errors)
+ [Batch Transform Sample Notebooks](#batch-transform-notebooks)
+ [Associate Prediction Results with Input Records](batch-transform-data-processing.md)

## Use Batch Transform to Get Inferences from Large Datasets<a name="batch-transform-large-datasets"></a>

Batch transform automatically manages the processing of large datasets within the limits of specified parameters\. For example, suppose that you have a dataset file, `input1.csv`, stored in an S3 bucket\. The content of the input file might look like the following\.: 

```
Record1-Attribute1, Record1-Attribute2, Record1-Attribute3, ..., Record1-AttributeM
Record2-Attribute1, Record2-Attribute2, Record2-Attribute3, ..., Record2-AttributeM
Record3-Attribute1, Record3-Attribute2, Record3-Attribute3, ..., Record3-AttributeM
...
RecordN-Attribute1, RecordN-Attribute2, RecordN-Attribute3, ..., RecordN-AttributeM
```

When a batch transform job starts, SageMaker initializes compute instances and distributes the inference or preprocessing workload between them\. Batch Transform partitions the Amazon S3 objects in the input by key and maps Amazon S3 objects to instances\. When you have multiples files, one instance might process `input1.csv`, and another instance might process the file named `input2.csv`\. 

To keep large payloads below the [ `MaxPayloadInMB `](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB             ) limit, you can split an input file into several mini\-batches\. For example, you might create a mini\-batch from `input1.csv` by including only two of the records\.

```
Record3-Attribute1, Record3-Attribute2, Record3-Attribute3, ..., Record3-AttributeM
Record4-Attribute1, Record4-Attribute2, Record4-Attribute3, ..., Record4-AttributeM
```

**Note**  
SageMaker processes each input file separately\. It doesn't combine mini\-batches from different input files to comply with the [ `MaxPayloadInMB `](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB               ) limit\.

To split input files into mini\-batches, when you create a batch transform job, set the [ `SplitType`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformInput.html#SageMaker-Type-TransformInput-SplitType             ) parameter value to `Line`\. If `SplitType` is set to `None` or if an input file can't be split into mini\-batches, SageMaker uses the entire input file in a single request\. 

If the batch transform job successfully processes all of the records in an input file, it creates an output file with the same name and the `.out` file extension\. For multiple input files, such as `input1.csv` and `input2.csv`, the output files are named `input1.csv.out` and `input2.csv.out`\. The batch transform job stores the output files in the specified location in Amazon S3, such as `s3://awsexamplebucket/output/`\. 

The predictions in an output file are listed in the same order as the corresponding records in the input file\. The output file `input1.csv.out`, based on the input file shown earlier, would look like the following\.

```
Inference1-Attribute1, Inference1-Attribute2, Inference1-Attribute3, ..., Inference1-AttributeM
Inference2-Attribute1, Inference2-Attribute2, Inference2-Attribute3, ..., Inference2-AttributeM
Inference3-Attribute1, Inference3-Attribute2, Inference3-Attribute3, ..., Inference3-AttributeM
...
InferenceN-Attribute1, InferenceN-Attribute2, InferenceN-Attribute3, ..., InferenceN-AttributeM
```

To combine the results of multiple output files into a single output file, set the [ `AssembleWith`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformOutput.html#SageMaker-Type-TransformOutput-AssembleWith             ) parameter to `Line`\.

When the input data is very large and is transmitted using HTTP chunked encoding, to stream the data to the algorithm, set [ `MaxPayloadInMB`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB) to `0`\. Amazon SageMaker built\-in algorithms don't support this feature\.

For information about using the API to create a batch transform job, see the [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html) API\. For more information about the correlation between batch transform input and output objects, see [ `OutputDataConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OutputDataConfig.html)\. For an example of how to use batch transform, see [Step 6\.2: Deploy the Model with Batch Transform](ex1-batch-transform.md)\.

## Speed up a Batch Transform Job<a name="batch-transform-reduce-time"></a>

If you are using the [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html) API, you can reduce the time it takes to complete batch transform jobs by using optimal values for parameters such as [ `MaxPayloadInMB`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB), [ `MaxConcurrentTransforms`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxConcurrentTransforms), or [ `BatchStrategy`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-BatchStrategy)\. If you are using the SageMaker console, you can specify these optimal parameter values in the **Additional configuration** section of the **Batch transform job configuration** page\. SageMaker automatically finds the optimal parameter settings for built\-in algorithms\. For custom algorithms, provide these values through an [execution\-parameters](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-batch-code.html#your-algorithms-batch-code-how-containe-serves-requests) endpoint\.

## Use Batch Transform to Test Production Variants<a name="batch-transform-test-variants"></a>

To test different models or various hyperparameter settings, create a separate transform job for each new model variant and use a validation dataset\. For each transform job, specify a unique model name and location in Amazon S3 for the output file\. To analyze the results, use [Inference Pipeline Logs and Metrics](inference-pipeline-logs-metrics.md)\.

## Batch Transform Errors<a name="batch-transform-errors"></a>

SageMaker uses the Amazon S3 [Multipart Upload API](https://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html) to upload results from a batch transform job to Amazon S3\. If an error occurs, the uploaded results are removed from Amazon S3\. In some cases, such as when a network outage occurs, an incomplete multipart upload might remain in Amazon S3\. To avoid incurring storage charges, we recommend that you add the [S3 bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html#mpu-abort-incomplete-mpu-lifecycle-config) to the S3 bucket lifecycle rules\. This policy deletes incomplete multipart uploads that might be stored in the S3 bucket\. For more information, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html)\.

If a batch transform job fails to process an input file because of a problem with the dataset, SageMaker marks the job as `failed`\. If an input file contains a bad record, the transform job doesn't create an output file for that input file because doing so prevents it from maintaining the same order in the transformed data as in the input file\. When your dataset has multiple input files, a transform job continues to process input files even if it fails to process one\. The processed files still generate useable results\.

Exceeding the [ `MaxPayloadInMB`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#SageMaker-CreateTransformJob-request-MaxPayloadInMB) limit causes an error\. This might happen with a large dataset if it can't be split, the [ `SplitType`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformInput.html#SageMaker-Type-TransformInput-SplitType) parameter is set to `none`, or individual records within the dataset exceed the limit\.

If you are using your own algorithms, you can use placeholder text, such as `ERROR`, when the algorithm finds a bad record in an input file\. For example, if the last record in a dataset is bad, the algorithm places the placeholder text for that record in the output file\.

## Batch Transform Sample Notebooks<a name="batch-transform-notebooks"></a>

For a sample notebook that uses batch transform with a principal component analysis \(PCA\) model to reduce data in a user\-item review matrix, followed by the application of a density\-based spatial clustering of applications with noise \(DBSCAN\) algorithm to cluster movies, see [Batch Transform with PCA and DBSCAN Movie Clusters](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_batch_transform/introduction_to_batch_transform/batch_transform_pca_dbscan_movie_clusters.ipynb)\. For instructions on creating and accessing Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After creating and opening a notebook instance, choose the **SageMaker Examples** tab to see a list of all the SageMaker examples\. The topic modeling example notebooks that use the NTM algorithms are located in the **Advanced functionality** section\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.