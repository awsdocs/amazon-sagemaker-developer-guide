# Give users permissions to import Amazon Redshift data<a name="canvas-redshift-permissions"></a>

Your users might have datasets stored in Amazon Redshift\. Before users can import data from Amazon Redshift into SageMaker Canvas, you must add the `AmazonRedshiftFullAccess` managed policy to the IAM execution role that you've used for the user profile\.

To add the `AmazonRedshiftFullAccess` policy to the user's IAM role, do the following\.

1. Go to the [IAM console](https://console.aws.amazon.com/iamv2)\.

1. Choose **Roles**\.

1. In the search box, search for the user's IAM role by name and select it\.

1. On the page for the user's role, under **Permissions**, choose **Add permissions**\.

1. Choose **Attach policies**\.

1. Search for the `AmazonRedshiftFullAccess` managed policy and select it\.

1. Choose **Attach policies** to attach the policy to the role\.

For more information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *AWS IAM User Guide*\.