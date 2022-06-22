# Set up SageMaker Canvas for your users<a name="setting-up-canvas-sso"></a>

To set up Amazon SageMaker Canvas for SSO with Okta, do the following:
+ Create an Amazon SageMaker Domain\.
+ Create user profiles for the SageMaker Domain\.
+ Set up Okta Single Sign On \(SSO\) for your users\.
+ Activate link sharing for models\.

NOTE:  If you instead wish to setup SageMaker Canvas with AWS SSO, please follow the directions outlined in [Onboard to Amazon SageMaker Domain Using AWS SSO](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-sso-users.html).  Once the domain has been onboarded, add SageMaker as an Application from within AWS SSO. From there you can add AWS SSO users or groups directly from the SageMaker Domain Control Panel.  For step-by-step instructions, please see the blog post [Enable business analysts to access Amazon SageMaker Canvas without using the AWS Management Console with AWS SSO](https://aws.amazon.com/blogs/machine-learning/enable-business-analysts-to-access-amazon-sagemaker-canvas-without-using-the-aws-management-console-with-aws-sso/).

Use Okta Single\-Sign On \(SSO\) to give your users access to Amazon SageMaker Canvas\. SageMaker Canvas supports SAML 2\.0 SSO methods\. The following sections guide you through procedures to set up Okta SSO\.

To set up an Amazon SageMaker Domain, see [Onboard to Amazon SageMaker Studio Using IAM](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-iam.html)\. You can use the following information to help you complete the procedure in the section:
+ You can ignore the step about creating projects\.
+ You don't need to provide access to additional Amazon S3 buckets\. Your users can use the default bucket that we provide when we create a role\.
+ To give your users the ability to share their notebooks with data scientists, turn on **Notebook Sharing Configuration**\.
+ Use Amazon SageMaker Studio version 3\.19\.0 or later\. For information about updating Amazon SageMaker Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

Use the following procedure to set up Okta\. For all of the following procedures, you specify the same IAM role for `IAM-role` \.

## Add SageMaker Canvas Application To Okta<a name="canvas-set-up-okta"></a>

Set up the sign\-on method for Okta\.

1. Sign in to the Okta Admin dashboard\.

1. Choose **Add application**\. Search for **AWS Account Federation**\.

1. Choose **Add**\.

1. Optional: Change the name to **Amazon SageMaker Canvas**\.

1. Choose **Next**\.

1. Choose **SAML 2\.0** as the **Sign\-On** method\.

1. Choose **Identity Provider Metadata** to open the metadata XML file\. Save the file locally\.

1. Choose **Done**\.

## Set Up ID Federation in IAM<a name="set-up-id-federation-IAM"></a>

AWS Identity and Access Management \(IAM\) is the AWS service that you use to gain access to your AWS account\. You gain access to AWS through an IAM account\.

1. Sign in to the AWS console\.

1. Choose **AWS Identity and Access Management \(IAM\)**\.

1. Choose **Identity Providers**\.

1. Choose **Create Provider**\.

1. For **Configure Provider**, specify the following:
   + **Provider Type** – From the dropdown menu, choose **SAML**\.
   + **Provider Name** – Specify **Okta**\.
   + **Metadata Document** – Upload the XML document that you've saved locally from step 7 of [Add SageMaker Canvas Application To Okta](#canvas-set-up-okta)\.

1. Find your identity provider under **Identity Providers**\. Copy its **Provider ARN** value\.

1. For **Roles**, choose the IAM role that you're using for Okta SSO access\.

1. Under **Trust Relationship** for the IAM role, choose **Edit Trust Relationship**\.

1. Modify the IAM trust relationship policy by specifying the **Provider ARN** value that you've copied and add the following policy:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Federated": "arn:aws:iam::123456789012:saml-provider/Okta"
         },
         "Action": [
           "sts:AssumeRoleWithSAML",
           "sts:SetSourceIdentity",
           "sts:TagSession"
         ],
         "Condition": {
           "StringEquals": {
             "SAML:aud": "https://signin.aws.amazon.com/saml"
           }
         }
       }
     ]
   }
   ```

1. For **Permissions**, add the following policy:

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AmazonSageMakerPresignedUrlPolicy",
              "Effect": "Allow",
              "Action": [
                   "sagemaker:CreatePresignedDomainUrl",
                   "sagemaker:CreatePresignedDomainUrlWithPrincipalTag"
              ],
              "Resource": "*"
         }
     ]
   }
   ```

## Configure SageMaker Canvas in Okta<a name="canvas-configure-okta"></a>

The following procedure gives you the ability to configure Amazon SageMaker Canvas in Okta\.

To configure Amazon SageMaker Canvas to use Okta, follow the steps in this section\. You must specify unique user names for each **SageMakerStudioProfileName** field\. For example, you can use `user.login` as a value\. If the username is different from the SageMaker Canvas profile name, choose a different uniquely identifying attribute\. For example, you can use an employee's ID number for the profile name\.

For an example of values that you can set for **Attributes**, see the code following the procedure\.

1. Under **Directory**, choose **Groups**\.

1. Add a group with the following pattern: `sagemaker#canvas#IAM-role#AWS-account-id`

1. In Okta, open the **AWS Account Federation** app integration configuration\.

1. Select **Sign On** for the AWS Account Federation app\.

1. Choose **Edit** and specify the following:
   + SAML 2\.0
   + **Default Relay State** – https://*Region*\.console\.aws\.amazon\.com/sagemaker/home?region=*Region*\#/studio/canvas/open/*StudioId*\. You can find the studio ID in the console: [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)

1. Choose **Attributes**\.

1. For the **SageMakerStudioProfileName** fields, specify unique values for each username\. The usernames must match the usernames that you've created in the AWS console\.

   ```
   Attribute 1:
   Name: https://aws.amazon.com/SAML/Attributes/PrincipalTag:SageMakerStudioUserProfileName 
   Value: ${user.login}
   
   Attribute 2:
   Name: https://aws.amazon.com/SAML/Attributes/TransitiveTagKeys
   Value: {"SageMakerStudioUserProfileName"}
   ```

1. Select **Environment Type**\. Choose **Regular AWS**\.
   +  If your environment type isn't listed, you can set your ACS URL in the **ACS URL** field\. If your environment type is listed, you don't need to enter your ACS URL

1. For **Identity Provider ARN**, specify the ARN you used in step 6 of the preceding procedure\.

1. Specify a **Session Duration**\.

1. Choose **Join all roles**\.

1. Turn on **Use Group Mapping** by specifying the following fields:
   + **App Filter** – `okta`
   + **Group Filter** – `^aws\#\S+\#(?IAM-role[\w\-]+)\#(?accountid\d+)$`
   + **Role Value Pattern** – `arn:aws:iam::$accountid:saml-provider/Okta,arn:aws:iam::$accountid:role/IAM-role`

1. Choose **Save/Next**\.

1. Under **Assignments**, assign the application to the group that you've created\.

## Add Optional Policies on Access Control in IAM<a name="canvas-optional-access"></a>

In IAM, you can apply the following policy to the administrator user who creates the user profiles\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CreateSageMakerStudioUserProfilePolicy",
            "Effect": "Allow",
            "Action": "sagemaker:CreateUserProfile",
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringEquals": {
                    "aws:TagKeys": [
                        "studiouserid"
                    ]
                }
            }
        }
    ]
}
```

If you choose to add the preceding policy to the admin user, you must use the following permissions from [Set Up ID Federation in IAM](#set-up-id-federation-IAM)\.

```
{
   "Version": "2012-10-17",
   "Statement": [
       {
           "Sid": "AmazonSageMakerPresignedUrlPolicy",
           "Effect": "Allow",
           "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:CreatePresignedDomainUrlWithPrincipalTag"
           ],
           "Resource": "*",
           "Condition": {
                  "StringEquals": {
                      "sagemaker:ResourceTag/studiouserid": "${aws:PrincipalTag/SageMakerStudioUserProfileName}"
                 }
            }
      }
  ]
}
```

## Activate link sharing for models<a name="canvas-set-up-link-sharing"></a>

In order to create shareable links for importing SageMaker Canvas models into SageMaker Studio, the SageMaker Domain must enable the notebook resource sharing option and have a valid Amazon S3 sharing location\. If you delete the Amazon S3 sharing location specified in your SageMaker Domain, or if you specify a non\-existent Amazon S3 location, you cannot create shareable links for SageMaker Canvas models\.

If you choose the **Quick setup** when creating your SageMaker Domain, SageMaker provides a default Amazon S3 location for resource sharing and automatically enables notebook resource sharing\.

If you choose the **Standard setup** when creating your SageMaker Domain, configure the following options when setting up the **Notebook Sharing Configuration**:

1. For **Shareable notebook resources**, turn on **Enable notebook resource sharing**\.

1. For **S3 location for shareable notebook resources**, enter either the default bucket already provided or enter a valid Amazon S3 path of your choice\.

The **Notebook Sharing Configuration** for your SageMaker Domain should look like the following screenshot, with **Enable notebook resource sharing** turned on and an Amazon S3 path entered for the **S3 location for shareable notebook resources** field\.

![\[Screenshot of the Notebook Sharing Configuration in the SageMaker Domain console with notebook resource sharing enabled and an example Amazon S3 location.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-notebook-sharing-config.png)
