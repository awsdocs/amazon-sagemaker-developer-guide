# Troubleshooting<a name="batch-transform-errors"></a>

If you are having errors in Amazon SageMaker Batch Transform, refer to the following troubleshooting tips\.

## Max timeout errors<a name="batch-transform-errors-max-timeout"></a>

If you are getting max timeout errors when running batch transform jobs, try the following:
+ Begin with the single\-record `[BatchStrategy](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#sagemaker-CreateTransformJob-request-BatchStrategy)`, a batch size of the default \(6 MB\) or smaller which you specify in the `[MaxPayloadInMB](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#sagemaker-CreateTransformJob-request-MaxPayloadInMB)` parameter, and a small sample dataset\. Tune the maximum timeout parameter `[InvocationsTimeoutInSeconds](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelClientConfig.html#sagemaker-Type-ModelClientConfig-InvocationsTimeoutInSeconds)` \(which has a maximum of 1 hour\) until you receive a successful invocation response\.
+ After you receive a successful invocation response, increase the `MaxPayloadInMB` \(which has a maximum of 100 MB\) and the `InvocationsTimeoutInSeconds` parameters together to find the maximum batch size that can support your desired model timeout\. You can use either the single\-record or multi\-record `BatchStrategy` in this step\.
**Note**  
Exceeding the `MaxPayloadInMB` limit causes an error\. This might happen with a large dataset if it can't be split, the `SplitType` parameter is set to none, or individual records within the dataset exceed the limit\.
+ \(Optional\) Tune the `[MaxConcurrentTransforms](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html#sagemaker-CreateTransformJob-request-MaxConcurrentTransforms)` parameter, which specifies the maximum number of parallel requests that can be sent to each instance in a batch transform job\. However, the value of `MaxConcurrentTransforms * MaxPayloadInMB` must not exceed 100 MB\.

## Incomplete output<a name="batch-transform-errors-incomplete"></a>

SageMaker uses the Amazon S3 [Multipart Upload API](https://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html) to upload results from a batch transform job to Amazon S3\. If an error occurs, the uploaded results are removed from Amazon S3\. In some cases, such as when a network outage occurs, an incomplete multipart upload might remain in Amazon S3\. An incomplete upload might also occur if you have multiple input files but some of the files can’t be processed by SageMaker Batch Transform\. The input files that couldn’t be processed won’t have corresponding output files in Amazon S3\.

To avoid incurring storage charges, we recommend that you add the [S3 bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html#mpu-abort-incomplete-mpu-lifecycle-config) to the S3 bucket lifecycle rules\. This policy deletes incomplete multipart uploads that might be stored in the S3 bucket\. For more information, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html)\.

## Job shows as `failed`<a name="batch-transform-errors-failed"></a>

If a batch transform job fails to process an input file because of a problem with the dataset, SageMaker marks the job as `failed`\. If an input file contains a bad record, the transform job doesn't create an output file for that input file because doing so prevents it from maintaining the same order in the transformed data as in the input file\. When your dataset has multiple input files, a transform job continues to process input files even if it fails to process one\. The processed files still generate useable results\.

If you are using your own algorithms, you can use placeholder text, such as `ERROR`, when the algorithm finds a bad record in an input file\. For example, if the last record in a dataset is bad, the algorithm places the placeholder text for that record in the output file\.