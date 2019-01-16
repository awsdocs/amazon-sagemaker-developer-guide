# Compile Machine Learning Models from the Amazon SageMaker Console<a name="neo-job-compilation-console"></a>

You can create a Neo compilation job in the Amazon SageMaker console\. In the **Amazon SageMaker** console, choose **Compilation jobs**, and then choose **Create compilation job**\.

![\[Create a compilation job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/8-create-compilation-job.png)

On the **Create compilation job** page, for **Job name**, enter a name\. Then select an **IAM role**\.

![\[IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/9-create-compilation-job-config.png)

If you don’t have an IAM role, choose **Create a new role**\.

![\[IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/10a-create-iam-role.png)

On the **Create an IAM role** page, choose **Any S3 bucket**, and choose **Create role**\.

![\[Create IAM role.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/10-create-iam-role.png)

In the **Input configuration** section, for **Location of model artifacts**, enter the path of the S3 bucket that contains your model artifacts\. For **Data input configuration**, enter the JSON string that specifies how many data matrix inputs you and the shape of each data matrices\. For **Machine learning framework**, choose the framework\.

![\[Input config.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/11b-create-compilation-job-input-config.png)

In the **Output configuration** section, for **S3 Output location**, enter the path to the S3 bucket or folder where you want to store the model\. For **Target device**, choose which device you want to deploy your model to, and choose **Create job**\.

![\[Create job.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/11c-create-compilation-job-output-config.png)

Check the status of the compilation job when started\.

![\[Compilation job status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/12-run-model-compilation.png)

Check the status of the compilation job when completed\.

![\[Compilation job status.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/12a-completed-model-compilation.png)