# Troubleshoot Neo Compilation Errors<a name="neo-troubleshooting-compilation"></a>

This section contains information about how to understand and prevent common compilation errors, the error messages they generate, and guidance on how to resolve these errors\. 

**Topics**
+ [How to Use This Page](#neo-troubleshooting-compilation-how-to-use)
+ [Framework\-Related Errors](#neo-troubleshooting-compilation-framework-related-errors)
+ [Infrastructure\-Related Errors](#neo-troubleshooting-compilation-infrastructure-errors)
+ [Check your compilation log](#neo-troubleshooting-compilation-logs)

## How to Use This Page<a name="neo-troubleshooting-compilation-how-to-use"></a>

Attempt to resolve your error by the going through these sections in the following order:

1. Check that the input of your compilation job satisfies the input requirements\. See [What input data shapes does SageMaker Neo expect?](neo-compilation-preparing-model.md#neo-job-compilation-expected-inputs)

1.  Check common [framework\-specific errors](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting-compilation.html#neo-troubleshooting-compilation-framework-related-errors)\. 

1.  Check if your error is an [infrastructure error](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting-compilation.html#neo-troubleshooting-compilation-infrastructure-errors)\. 

1. Check your [compilation log](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting-compilation.html#neo-troubleshooting-compilation-logs)\.

## Framework\-Related Errors<a name="neo-troubleshooting-compilation-framework-related-errors"></a>

### TensorFlow<a name="neo-troubleshooting-compilation-framework-related-errors-tensorflow"></a>


| Error | Solution | 
| --- | --- | 
|   `InputConfiguration: Exactly one .pb file is allowed for TensorFlow models.`   |  Make sure you only provide one \.pb or \.pbtxt file\.  | 
|  `InputConfiguration: Exactly one .pb or .pbtxt file is allowed for TensorFlow models.`  |  Make sure you only provide one \.pb or \.pbtxt file\.  | 
|   ` ClientError: InputConfiguration: TVM cannot convert <model zoo> model. Please make sure the framework you selected is correct. The following operators are not implemented: {<operator name>} `   |   Check the operator you chose is supported\. See [SageMaker Neo Supported Frameworks and Operators](http://aws.amazon.com/releasenotes/sagemaker-neo-supported-frameworks-and-operators/)\.   | 

### Keras<a name="neo-troubleshooting-compilation-framework-related-errors-keras"></a>


| Error | Solution | 
| --- | --- | 
|   `InputConfiguration: No h5 file provided in <model path>`   |   Check your h5 file is in the Amazon S3 URI you specified\.  *Or* Check that the [h5 file is correctly formatted](https://www.tensorflow.org/guide/keras/save_and_serialize#keras_h5_format)\.   | 
|   `InputConfiguration: Multiple h5 files provided, <model path>, when only one is allowed`   |  Check you are only providing one `h5` file\.  | 
|   `ClientError: InputConfiguration: Unable to load provided Keras model. Error: 'sample_weight_mode'`   |  Check the Keras version you specified is supported\. See, supported frameworks for [cloud instances](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-supported-cloud.html) and [edge devices](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-supported-devices-edge.html)\.   | 
|   `ClientError: InputConfiguration: Input input has wrong shape in Input Shape dictionary. Input shapes should be provided in NCHW format. `   |   Check that your model input follows NCHW format\. See [What input data shapes does SageMaker Neo expect?](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-job-compilation.html#neo-job-compilation-expected-inputs)   | 

### MXNet<a name="neo-troubleshooting-compilation-framework-related-errors-mxnet"></a>


| Error | Solution | 
| --- | --- | 
|   `ClientError: InputConfiguration: Only one parameter file is allowed for MXNet model. Please make sure the framework you select is correct.`   |   SageMaker Neo will select the first parameter file given for compilation\.   | 

## Infrastructure\-Related Errors<a name="neo-troubleshooting-compilation-infrastructure-errors"></a>


| Error | Solution | 
| --- | --- | 
|   `ClientError: InputConfiguration: S3 object does not exist. Bucket: <bucket>, Key: <bucket key>`   |  Check the Amazon S3 URI your provided\.  | 
|   ` ClientError: InputConfiguration: Bucket <bucket name> is in region <region name> which is different from AWS Sagemaker service region <service region> `   |   Create an Amazon S3 bucket that is in the same region as the service\.   | 
|   ` ClientError: InputConfiguration: Unable to untar input model. Please confirm the model is a tar.gz file `   |   Check that your model in Amazon S3 is compressed into a `tar.gz` file\.   | 

## Check your compilation log<a name="neo-troubleshooting-compilation-logs"></a>

1. Navigate to Amazon CloudWatch at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Select the region you created the compilation job from the **Region** dropdown list in the top right\.

1. In the navigation pane of the Amazon CloudWatch, choose **Logs**\. Select **Log groups**\.

1. Search for the log group called `/aws/sagemaker/CompilationJobs`\. Select the log group\.

1. Search for the logstream named after the compilation job name\. Select the log stream\.