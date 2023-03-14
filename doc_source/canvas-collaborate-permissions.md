# Grant Users Permissions to Collaborate with Studio<a name="canvas-collaborate-permissions"></a>

Your Amazon SageMaker Canvas users might want to share their models with users in Amazon SageMaker Studio to receive feedback and model updates, and Studio users might want to share models with Canvas users so that they can generate predictions in Canvas\. The following permissions grant Canvas users and Studio users access to share models with each other\.

For more information about how Canvas users can share models with Studio users, see [Collaborate with data scientists](canvas-collaborate.md)\. For more information about how Canvas users can bring a model shared from Studio, see [Bring your own model to SageMaker Canvas](canvas-byom.md)\.

Before Canvas and Studio users can collaborate, the users must be in the same Amazon SageMaker Domain\. Add the following IAM permissions added to the same IAM execution role that you've used for their profiles\.

To add the permissions to the users’ IAM role, do the following:

1. Go to the [IAM console](https://console.aws.amazon.com/iamv2)\.

1. Choose **Roles**\.

1. In the search box, search for the user's IAM role by name and select it\.

1. On the page for the user's role, under **Permissions**, choose **Add permissions**\.

1. Choose **Attach policies**\.

1. Enter the following IAM policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "sagemaker:CreateSharedModel",
                   "sagemaker:DescribeSharedModel",
                   "sagemaker:ListSharedModelEvents",
                   "sagemaker:ListSharedModels",
                   "sagemaker:ListSharedModelVersions",
                   "sagemaker:SendSharedModelEvent",
                   "sagemaker:UpdateSharedModel",
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Attach policies** to attach the policy to the role\.

For more information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\.