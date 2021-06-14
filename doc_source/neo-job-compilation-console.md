# Compile a Model \(Amazon SageMaker Console\)<a name="neo-job-compilation-console"></a>

You can create an Amazon SageMaker Neo compilation job in the Amazon SageMaker console\.

1. In the **Amazon SageMaker** console, choose **Compilation jobs**, and then choose **Create compilation job**\.  
![\[Create a compilation job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/8-create-compilation-job.png)

1. On the **Create compilation job** page, under **Job name**, enter a name\. Then select an **IAM role**\.  
![\[IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/9-create-compilation-job-config.png)

1. If you don’t have an IAM role, choose **Create a new role**\.  
![\[IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/10a-create-iam-role.png)

1. On the **Create an IAM role** page, choose **Any S3 bucket**, and choose **Create role**\.  
![\[Create IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/10-create-iam-role.png)

1. 

------
#### [ Non PyTorch Frameworks ]

   Within the **Input configuration** section, enter the full path of the Amazon S3 bucket URI that contains your model artifacts in the **Location of model artifacts** input field\. Your model artifacts must be in a compressed tarball file format \(`.tar.gz`\)\. 

    For the **Data input configuration** field, enter the JSON string that specifies the shape of the input data\. For **Machine learning framework**, choose the framework of your choice\.

![\[Input config.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/neo-create-compilation-job-input-config.png)

   To find the JSON string examples of input data shapes depending on frameworks, see [What input data shapes Neo expects](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting.html#neo-troubleshooting-errors-preventing)\.

------
#### [ PyTorch Framework ]

   Similar instructions apply for compiling PyTorch models\. However, if you trained with PyTorch and are trying to compile the model for `ml_*` \(except `ml_inf`\) target, you can optionally specify the version of PyTorch you used\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/compile_console_pytorch.png)

   To find the JSON string examples of input data shapes depending on frameworks, see [What input data shapes Neo expects](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting.html#neo-troubleshooting-errors-preventing)\.

**Note**  
When compiling for `ml_*` instances using PyTorch framework, use **Compiler options** field in **Output Configuration** to provide the correct data type \(`dtype`\) of the model’s input\. The default is set to `"float32"`\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/neo_compilation_console_pytorch_compiler_options.png)

**Warning**  
 If you specify a Amazon S3 bucket URI path that leads to `.pth` file, you will receive the following error after starting compilation: `ClientError: InputConfiguration: Unable to untar input model.Please confirm the model is a tar.gz file` 

------

1.  Go to the **Output configuration** section\. Choose where you want to deploy your model\. You can deploy your model to a **Target device** or a **Target platform**\. Target devices include cloud and edge devices\. Target platforms refer to specific OS, architecture, and accelerators you want your model to run on\. 

    For **S3 Output location**, enter the path to the S3 bucket where you want to store the model\. You can optionally add compiler options in JSON format under the **Compiler options** section\.   
![\[Create job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/neo-console-output-config.png)

1. Check the status of the compilation job when started\. This status of the job can be found at the top of the **Compilation Job** page, as shown in the following screenshot\. You can also check the status of it in the **Status** column\.  
![\[Compilation job status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/12-run-model-compilation.png)

1. Check the status of the compilation job when completed\. You can check the status in the **Status** column as shown in the following screenshot\.  
![\[Compilation job status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/12a-completed-model-compilation.png)