# Share Data for Your Amazon S3 Bucket<a name="gts-share-data"></a>

After an AWS expert reaches out to discuss your project, you may be required to fill out an intake form with questions specific to your synthetic data requirements\. You receive an *aws\-account\-id* from the AWS expert once your project is approved\. You use this *aws\-account\-id* to create an Amazon S3 bucket for storing your output data\.

**To create an Amazon S3 bucket and share it with us:**

1. Follow the instructions in [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. We recommend using the following naming convention while storing your data in an Amazon S3 bucket\.

   1. The *project\-name* should contain fewer than 63 characters\.

   1. The *project\-name* can include hyphens, but no spaces and underscores\.

1. In the **Buckets** list, choose the name of the bucket you created\.

1. Choose **Permissions**\.

1. In the **Bucket policy** section, choose **Edit**\.

1. Copy the following [policy](https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy.html) to allow the operators to access your Amazon S3 bucket and enter the *aws\-account\-id* and *your\-bucket\-name*\.

   ```
    {
       "Version": "2008-10-17",
       "Statement": [
       {
           "Sid": "DataSyncCreateS3LocationAndTaskAccess",
           "Effect": "Allow",
           "Principal": {
           "AWS": "arn:aws:iam::aws-account-id:role/B12CopyToCustomerBucketDataSyncRole"
           },
           "Action": [
               "s3:GetBucketLocation",
               "s3:ListBucket",
               "s3:ListBucketMultipartUploads",
               "s3:AbortMultipartUpload",
               "s3:DeleteObject",
               "s3:GetObject",
               "s3:ListMultipartUploadParts",
               "s3:PutObject",
               "s3:GetObjectTagging",
               "s3:PutObjectTagging"
           ],
           "Resource": [
               "arn:aws:s3:::your-bucket-name",
               "arn:aws:s3:::your-bucket-name/*"
           ]
       },
       {
           "Sid": "DataSyncCreateS3Location",
           "Effect": "Allow",
           "Principal": {
           "AWS": "arn:aws:iam::aws-account-id:role/B12StudioOperator"
           },
           "Action": [
               "s3:ListBucket",
               "s3:PutObject"
           ],
           "Resource": [
               "arn:aws:s3:::your-bucket-name",
               "arn:aws:s3:::your-bucket-name/*"
             ]
           }
       ]
       
   }
   ```
**Note**  
The **Object Ownership** should have ACLs disabled as shown in the image below\.  
![\[Screenshot of the Amazon S3 console with ACLs disabled.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gts-share-data.png)

1. Choose **Save changes**\.

**Note**  
If you have additional requirements for accessing your data in an Amazon S3 bucket, please contact your AWS expert\. 

After receiving your intake form, we return a statement of work \(SOW\) within 5 business days\. The SOW outlines your engagement with Ground Truth synthetic data generation and labeling\. After you approve the SOW, Ground Truth begins the production of a test batch consisting of 50 synthetic images\. An AWS expert meets with you to the review the test batch, approve or reject images, and complete the final production\. The timeline for this is based on the responses in your intake form\.