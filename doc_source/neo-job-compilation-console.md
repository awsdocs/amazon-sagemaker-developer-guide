# Compile a Model \(Amazon SageMaker Console\)<a name="neo-job-compilation-console"></a>

You can create a Neo compilation job in the Amazon SageMaker console\.

1. In the **Amazon SageMaker** console, choose **Compilation jobs**, and then choose **Create compilation job**\.  
![\[Create a compilation job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/8-create-compilation-job.png)

1. On the **Create compilation job** page, for **Job name**, enter a name\. Then select an **IAM role**\.  
![\[IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/9-create-compilation-job-config.png)

1. If you don’t have an IAM role, choose **Create a new role**\.  
![\[IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/10a-create-iam-role.png)

1. On the **Create an IAM role** page, choose **Any S3 bucket**, and choose **Create role**\.  
![\[Create IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/10-create-iam-role.png)

1. Within the **Input configuration** section, enter the path of the S3 bucket that contains your model artifacts in the **Location of model artifacts** input field\. For the **Data input configuration** field, enter the JSON string that specifies the shape of the input data\. For **Machine learning framework**, choose the framework of your choice\.  
![\[Input config.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/11b-create-compilation-job-input-config.png)

   To find the JSON string examples of input data shapes depending on frameworks, see [What input data shapes Neo expects](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-troubleshooting.html#neo-troubleshooting-errors-preventing)\.

1. Go to the **Output configuration** section\. For **S3 Output location**, enter the path to the S3 bucket where you want to store the model\. For **Target device**, choose which device you want to deploy your model to, and choose **Create job**\.  
![\[Create job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/11c-create-compilation-job-output-config.png)

1. Check the status of the compilation job when started\. This status of the job can be found at the top of the Compilation Job page as shown in the following screenshot\. You can also check the status of it in the **Status** column\.  
![\[Compilation job status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/12-run-model-compilation.png)

1. Check the status of the compilation job when completed\. You can check the status in the **Status** column as shown in the following screenshot\.  
![\[Compilation job status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/12a-completed-model-compilation.png)