# Grant Users Permissions to Import Amazon Redshift Data<a name="canvas-redshift-permissions"></a>

Your users might have datasets stored in Amazon Redshift\. Before users can import data from Amazon Redshift into SageMaker Canvas, you must add the `AmazonRedshiftFullAccess` managed policy to the IAM execution role that you've used for the user profile and add Amazon Redshift as a service principal to the role's trust policy\. You must also associate the IAM execution role with your Amazon Redshift cluster\. Complete the procedures in the following sections to give your users the required permissions to import Amazon Redshift data\.

## Add Amazon Redshift permissions to your IAM role<a name="canvas-redshift-permissions-iam-role"></a>

You must grant Amazon Redshift permissions to the IAM role specified in your user profile\.

To add the `AmazonRedshiftFullAccess` policy to the user's IAM role, do the following\.

1. Sign in to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles**\.

1. In the search box, search for the user's IAM role by name and select it\.

1. On the page for the user's role, under **Permissions**, choose **Add permissions**\.

1. Choose **Attach policies**\.

1. Search for the `AmazonRedshiftFullAccess` managed policy and select it\.

1. Choose **Attach policies** to attach the policy to the role\.

After attaching the policy, the role’s **Permissions** section should now include `AmazonRedshiftFullAccess`\.

To add Amazon Redshift as a service principal to the IAM role, do the following\.

1. On the same page for the IAM role, under **Trust relationships**, choose **Edit trust policy**\.

1. In the **Edit trust policy** editor, update the trust policy to add Amazon Redshift as a service principal\. An IAM role that allows Amazon Redshift to access other AWS services on your behalf has a trust relationship as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "redshift.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. After editing the trust policy, choose **Update policy**\.

You should now have an IAM role that has the policy `AmazonRedshiftFullAccess` attached to it and a trust relationship established with Amazon Redshift, giving users permission to import Amazon Redshift data into SageMaker Canvas\. For more information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\.

## Associate the IAM role with your Amazon Redshift cluster<a name="canvas-redshift-permissions-cluster"></a>

In the settings for your Amazon Redshift cluster, you must associate the IAM role that you granted permissions to in the preceding section\.

To associate an IAM role with your cluster, do the following\.

1. Sign in to the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, and then choose the name of the cluster that you want to update\.

1. In the **Actions** dropdown menu, choose **Manage IAM roles**\. The **Cluster permissions** page appears\.

1. For **Available IAM roles**, enter either the ARN or the name of the IAM role, or choose the IAM role from the list\.

1. Choose **Associate IAM role** to add it to the list of **Associated IAM roles**\.

1. Choose **Save changes** to associate the IAM role with the cluster\.

Amazon Redshift modifies the cluster to complete the change, and the IAM role to which you previously granted Amazon Redshift permissions is now associated with your Amazon Redshift cluster\. Your users now have the required permissions to import Amazon Redshift data into SageMaker Canvas\.