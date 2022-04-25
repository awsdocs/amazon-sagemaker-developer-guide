# Prerequisites<a name="serverless-endpoints-prerequisites"></a>

Before you can create a serverless endpoint, complete the following prerequisites\.

1. **Set up an AWS account\.** You first need an AWS account and an AWS Identity and Access Management administrator user\. For instructions on how to set up an AWS account, see [How do I create and activate a new AWS account?](http://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)\. For instructions on how to secure your account with an IAM administrator user, see [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.

1. **Create an Amazon S3 bucket\.** You use an Amazon S3 bucket to store your model artifacts\. To learn how to create a bucket, see [Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html) in the *Amazon S3 User Guide*\.

1. **Upload your model artifacts to your S3 bucket\.** For instructions on how to upload your model to your bucket, see [Upload an object to your bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/uploading-an-object-bucket.html) in the *Amazon S3 User Guide*\.

1. **Create an IAM role for Amazon SageMaker\.** Amazon SageMaker needs access to the S3 bucket that stores your model\. Create an IAM role with a policy that gives SageMaker read access to your bucket\. The following procedure shows how to create a role in the console, but you can also use the [CreateRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateRole.html) API from the *IAM User Guide*\. For information on giving your role more granular permissions based on your use case, see [SageMaker Roles](sagemaker-roles.md#sagemaker-roles-createmodel-perms)\.

   1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/)\.

   1. In the navigation tab, choose **Roles**\.

   1. Choose **Create Role**\.

   1. For **Select type of trusted entity**, choose **AWS service** and then choose **SageMaker**\.

   1. Choose **Next: Permissions** and then choose **Next: Tags**\.

   1. \(Optional\) Add tags as key\-value pairs if you want to have metadata for the role\.

   1. Choose **Next: Review**\.

   1.  For **Role name**, enter a name for the new role that is unique within your AWS account\. You cannot edit the role name after creating the role\.

   1. \(Optional\) For **Role description**, enter a description for the new role\.

   1. Choose **Create role**\.

1. **Attach S3 bucket permissions to your SageMaker role\.** After creating an IAM role, attach a policy that gives SageMaker permission to access the S3 bucket containing your model artifacts\.

   1. In the IAM console navigation tab, choose **Roles**\.

   1. From the list of roles, search for the role you created in the previous step by name\.

   1. Choose your role, and then choose **Attach policies**\.

   1. For **Attach permissions**, choose **Create policy**\.

   1. In the **Create policy** view, select the **JSON** tab\.

   1. Add the following policy statement into the JSON editor\. Make sure to replace `<your-bucket-name>` with the name of the S3 bucket that stores your model artifacts\. If you want to restrict the access to a specific folder or file in your bucket, you can also specify the Amazon S3 folder path, for example, `<your-bucket-name>/<model-folder>`\.

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Sid": "VisualEditor0",
                  "Effect": "Allow",
                  "Action": "s3:GetObject",
                  "Resource": "arn:aws:s3:::<your-bucket-name>/*"
              }
          ]
      }
      ```

   1. Choose **Next: Tags**\.

   1. \(Optional\) Add tags in key\-value pairs to the policy\.

   1. Choose **Next: Review**\.

   1. For **Name**, enter a name for the new policy\.

   1. \(Optional\) Add a **Description** for the policy\.

   1. Choose **Create policy**\.

   1. After creating the policy, return to **Roles** in the [IAM console](https://console.aws.amazon.com/iam/) and select your SageMaker role\.

   1. Choose **Attach policies**\.

   1. For **Attach permissions**, search for the policy you created by name\. Select it and choose **Attach policy**\.

1. **Select a prebuilt Docker container image or bring your own\.** The container you choose serves inference on your endpoint\. SageMaker provides containers for built\-in algorithms and prebuilt Docker images for some of the most common machine learning frameworks, such as Apache MXNet, TensorFlow, PyTorch, and Chainer\. For a full list of the available SageMaker images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\.

   If none of the existing SageMaker containers meet your needs, you may need to create your own Docker container\. For information about how to create your Docker image and make it compatible with SageMaker, see [Use Your Own Inference Code](your-algorithms-inference-main.md)\. To use your container with a serverless endpoint, the container image must reside in an Amazon ECR repository within the same AWS account that creates the endpoint\.

1. **\(Optional\) Register your model with Model Registry\.** [SageMaker Model Registry](model-registry.md) helps you catalog and manage versions of your models for use in ML pipelines\. For more information about registering a version of your model, see [Create a Model Group](model-registry-model-group.md) and [Register a Model Version](model-registry-version.md)\. For an example of a Model Registry and Serverless Inference workflow, see the following [example notebook](https://github.com/aws/amazon-sagemaker-examples/blob/main/serverless-inference/serverless-model-registry.ipynb)\.