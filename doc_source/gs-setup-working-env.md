# Step 2: Create an Amazon SageMaker Notebook Instance<a name="gs-setup-working-env"></a>

An Amazon SageMaker notebook instance is a fully managed machine learning \(ML\) EC2 compute instance running the Jupyter Notebook App\. For more information, see [Notebook Instances and Notebooks](how-it-works-notebooks-instances.md)\. 

**Note**  
If necessary, you can change the notebook instance settings, including the ML compute instance type, later\.

**To create an Amazon SageMaker notebook instance**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook instances**, and then choose **Create notebook instance**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-workspace-10.png)

1. On the **Create notebook instance** page, provide the following information: 

   + For **Notebook instance name**, type **ExampleNotebookInstance**\.

   + For **Instance type**, choose **ml\.t2\.medium**\.

   + For **IAM role**, create an IAM role\.

     1. Choose **Create a new role**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-workspace-20.png)

     1. Choose additional S3 buckets that you want to use with Amazon SageMaker\. 

         

        In the preceding section, you created a bucket with `sagemaker` in its name\. This IAM role automatically has the S3 permissions to use that bucket through the `AmazonSageMakerFullAccess` policy that Amazon SageMaker attaches to the role\. The bucket you created is sufficient for the model training exercise in Getting Started\. However, as you explore Amazon SageMaker, you might access other S3 buckets from your notebook instance\. Amazon SageMaker will need permissions to access those buckets\.

         

        You can also choose to access additional S3 buckets from your Amazon SageMaker notebook instance:

         

        + If you're not concerned about users in your AWS account accessing your data, choose **Any S3 bucket**\.

           

        + If your account has sensitive data \(such as Human Resources information\), restrict access by choosing **Specific S3 buckets**\. You can update the permissions policy attached to the role you are creating later\.

           

        + Restrict access by choosing **None**\. To explicitly control access, use bucket and object names and tags as supported by the `AmazonSageMakerFullAccess` policy\. For more information, see [Using the AWS Managed Permission Policy \(AmazonSageMakerFullAccess\) for an Execution Role](sagemaker-roles.md#sagemaker-roles-amazonsagemakerfullaccess-policy)\.

         

     1. Choose **Create role**\.

        Amazon SageMaker creates an IAM role\. The role name has this form: `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. For example, `AmazonSageMaker-ExecutionRole-20171125T090800`\.

         

        You can view this role in the IAM console\. Observe the following policies attached to the role:

         

        + The trust policy allows Amazon SageMaker to assume the role\. 

        + The role has the `AmazonSageMakerFullAccess` AWS managed policy attached\. 

           

          If you specified additional S3 bucket\(s\) when creating the IAM role, view the customer managed policy attached to the role\. The name of the customer managed policy has this form: `AmazonSageMaker-ExecutionPolicy-YYYYMMDDTHHmmSS`\. 

        For more information about creating your own IAM role, see [Amazon SageMaker Roles ](sagemaker-roles.md)\. 

         

   + Choosing a Virtual Private Cloud \(VPC\) is optional for this exercise\. 
**Note**  
If you want to access resources in your VPC from the notebook instance, choose a VPC and a **SubnetId**\. For **Security Group**, choose the default security group of the selected VPC\. The inbound and outbound rules of the default security group are sufficient for the exercises in this guide\.   
To connect to a resource in your VPC, the resource must resolve to a private IP address in your VPC\. For example, to ensure that an Amazon Redshift DNS name resolves to a private IP address, ensure one of the following:   
The Amazon Redshift cluster is not publicly accessible\.
If the Amazon Redshift cluster is publicly accessible, set the `DNS resolution` and `DNS hostnames` VPC parameters to `true`\. For information on how to set those parameters, see [Managing Clusters in an Amazon Virtual Private Cloud \(VPC\)](http://docs.aws.amazon.com//redshift/latest/mgmt/managing-clusters-vpc.html) 

   + Specifying a KMS encryption key is optional for this exercise\. Specify one if you want Amazon SageMaker to use the key to encrypt data in the ML storage volume attached to the notebook instance\.

1. Choose **Create notebook instance**\. 

   This launches a ML compute instance, in this case, a notebook instance, and attaches an ML storage volume to it\. For more information, see the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\. 

1. It takes a few minutes to create the notebook instance\. The notebook instance provides you with a preconfigured Jupyter notebook server and a set of Anaconda libraries\. When the status of the notebook instance is `InService`, choose **Open** next to the notebook instance's name\. The Juypter dashboard appears:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ironman-jupyter-home-page.png)

   The following are available in the dashboard:

   + In **Files**, there is a `sample_notebook` folder, which contains a number of sample notebooks\. For more information about them, see [GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\.

   + Amazon SageMaker provides several kernels for Jupyter, including ones that provide support for Python 2 and 3, MXNet, TensorFlow, and PySpark\. To choose a kernel for your notebook, use the **New** menu\. 

   For more information, see [The Jupyter notebook](https://jupyter-notebook.readthedocs.io/en/stable/)\.

## Next Step<a name="gs-setup-working-env-nextstep"></a>

You are now ready to train your first model\. For step\-by\-step instructions, see [Step 3: Train a Model with a Built\-in Algorithm and Deploy It](ex1.md)\.
