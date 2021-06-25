# Setting Up<a name="edge-getting-started-step1"></a>

Before you begin using SageMaker Edge Manager to manage models on your device fleets, you must first create IAM Roles for both SageMaker and AWS IoT\. You will also want to create at least one Amazon S3 bucket where you will store your pre\-trained model, the output of your SageMaker Neo compilation job, as well as input data from your edge devices\.

1. **Set up an AWS account\.**

   Create an AWS account and an IAM administrator user\. For instructions on how to set up your AWS account, see [How do I create and activate a new AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) For instructions on how to create an administrator user in your AWS account, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\.

1. **Create an IAM role for Amazon SageMaker\.**

   SageMaker Edge Manager needs access to your Amazon S3 bucket URI\. To facilitate this, create an IAM role that can run SageMaker and has permission to access Amazon S3\. Using this role, SageMaker can run under your account and access to your Amazon S3 bucket\.

   You can create an IAM role by using the IAM console, AWS SDK for Python \(Boto3\), or AWS CLI\. The following is an example of how to create an IAM role and attach the necessary policies with the IAM console\.

   1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

   1. For **Select type of trusted entity**, choose **AWS service**\.

   1. Choose the service that you want to allow to assume this role\. In this case, choose **SageMaker**\. Then choose **Next: Permissions**\.
      + This automatically creates an IAM policy that grants access to related services such as Amazon S3, Amazon ECR, and CloudWatch Logs\.

   1. Choose **Next: Tags**\.

   1. \(Optional\) Add metadata to the role by attaching tags as key–value pairs\. For more information about using tags in IAM, see [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html)\.

   1. Choose **Next: Review**\.

   1. Type in a **Role name**\. 

   1. If possible, type a role name or role name suffix\. Role names must be unique within your AWS account\. They are not distinguished by case\. For example, you cannot create roles named both `PRODROLE` and `prodrole`\. Because other AWS resources might reference the role, you cannot edit the name of the role after it has been created\.

   1. \(Optional\) For **Role description**, type a description for the new role\.

   1. Review the role and then choose **Create role**\.

      Note the SageMaker Role ARN, which you use to create a compilation job with SageMaker Neo and a packaging job with Edge Manager\. To find out the role ARN using the console, do the following:

      1. Go to the IAM console: [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)

      1. Select **Roles**\.

      1. Search for the role you just created by typing in the name of the role in the search field\.

      1. Select the role\.

      1. The role ARN is at the top of the **Summary** page\.

1. **Create an IAM role for AWS IoT\.**

   The AWS IoT IAM role you create is used to authorize your thing objects\. You also use the IAM role ARN to create and register device fleets with a SageMaker client object\.

   Configure an IAM role in your AWS account for the credentials provider to assume on behalf of the devices in your device fleet\. Then, attach a policy to authorize your devices to interact with AWS IoT services\.

   Create a role for AWS IoT either programmatically or with the IAM console, similar to what you did when you created a role for SageMaker\.

   1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

   1. For **Select type of trusted entity**, choose **AWS service**\.

   1. Choose the service that you want to allow to assume this role\. In this case, choose **IoT**\. Select **IoT** as the **Use Case**\.

   1. Choose **Next: Permissions**\.

   1. Choose **Next: Tags**\.

   1. \(Optional\) Add metadata to the role by attaching tags as key–value pairs\. For more information about using tags in IAM, see [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html)\.

   1. Choose **Next: Review**\.

   1. Type in a **Role name**\. The role name must start with `SageMaker`\.

   1. \(Optional\) For **Role description**, type a description for the new role\.

   1. Review the role and then choose **Create role**\.

   1. Once the role is created, choose **Roles** in the IAM console\. Search for the role you created by typing in role name in the **Search** field\.

   1. Choose your role\.

   1. Next, choose **Attach Policies**\.

   1. Search for `AmazonSageMakerEdgeDeviceFleetPolicy` in the **Search** field\. Select `AmazonSageMakerEdgeDeviceFleetPolicy`\.

   1. Choose **Attach policy**\.

   1. Add the following policy statement to the trust relationship:

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
            {
              "Effect": "Allow",
              "Principal": {"Service": "credentials.iot.amazonaws.com"},
              "Action": "sts:AssumeRole"
            },
            {
              "Effect": "Allow",
              "Principal": {"Service": "sagemaker.amazonaws.com"},
              "Action": "sts:AssumeRole"
            }
        ]
      }
      ```

      A trust policy is a [JSON policy document](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar) in which you define the principals that you trust to assume the role\. For more information about trust policies, see [Roles terms and concepts](IAM/latest/UserGuide/id_roles_terms-and-concepts.html)\.

   1. Note the AWS IoT role ARN\. You use the AWS IoT Role ARN to create and register the device fleet\. To find the IAM role ARN with the console:

      1. Go to the IAM console: [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)

      1. Choose **Roles**\.

      1. Search for the role you created by typing in the name of the role in the **Search** field\.

      1. Select the role\.

      1. The role ARN is on the Summary page\.

1. **Create an Amazon S3 bucket\.**

   SageMaker Neo and Edge Manager access your pre\-compiled model and compiled model from an Amazon S3 bucket\. Edge Manager also stores sample data from your device fleet in Amazon S3\.

   1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1. Choose **Create bucket**\.

   1. In **Bucket name**, enter a name for your bucket\.

   1. In **Region**, choose the AWS Region where you want the bucket to reside\.

   1. In **Bucket settings for Block Public Access**, choose the settings that you want to apply to the bucket\.

   1. Choose **Create bucket**\.

   For more information about creating Amazon S3 buckets, see [Getting started with Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html)\.

   Store the name of the bucket in the following variable:

   ```
   bucket=<bucket-name>
   ```