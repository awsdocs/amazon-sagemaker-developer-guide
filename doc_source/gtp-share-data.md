# Share Data with Amazon SageMaker Ground Truth Plus<a name="gtp-share-data"></a>

After the initial consultation call with the AWS expert, you can share your data in a secure Amazon S3 bucket to start the pilot\.

**To share data from your Amazon S3 bucket:**

1. Once your project is approved, ask your AWS expert for your Ground Truth Plus account ID\.

   Write down your Ground Truth Plus account ID since you will need it in the next step\.

1. Create an Amazon S3 bucket for storing your input and output data\. To create a bucket, follow the instructions in [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. 

1. We recommend using the following naming convention while storing your data in an Amazon S3 bucket\.

   1. The *project\-name* should contain fewer than 63 characters\.

   1. The *project\-name* can include hyphens, but no spaces and underscores\.

   1. Batches can be named as **batch1**, **batch2**, **batch3**, and so on\.

   1. Store your input data in the Amazon S3 bucket under the following prefix \(directory\):

      ```
       s3://your-bucket-name/ground-truth-plus/input/project-name/batch-name/..
      ```

   1. Ground Truth Plus currently accepts videos in \.mp4 format\.

   1. Video frames can be in \.jpg, \.jpeg, or \.png format\. The frames can be in a folder, sorted with UTF\-8 ordering\.

1. In the **Buckets** list, choose the name of the bucket you created\.

1. Choose **Permissions**\.

1. In the **Bucket policy** section, choose **Edit**\.

1. Copy the [policy](https://docs.aws.amazon.com/https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy.html) shown below and enter *your\-ground\-truth\-plus\-account\-id* and *your\-bucket\-name*

   ```
           {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "AWS": [your-ground-truth-plus-account-id]
                   // Ex: ["999999999999"]
               },
               "Action": [
                   "s3:GetObject",
                   "s3:GetBucketLocation",
                   "s3:ListBucket",
                   "s3:PutObject"
               ],
               "Resource": [
                   "arn:aws:s3:::your-bucket-name",
                   "arn:aws:s3:::your-bucket-name/*" 
                   //Ex: "arn:aws:s3:::input-data-to-label/*"
               ]
           }
       ]
   }
   ```

1. Choose **Save changes**\.

**Note**  
 If you have additional requirements for accessing your data in an Amazon S3 bucket, please contact your AWS expert\. 