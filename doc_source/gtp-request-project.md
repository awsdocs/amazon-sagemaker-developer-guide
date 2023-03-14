# Request a Project<a name="gtp-request-project"></a>

You can request a free of cost pilot by creating a project\.

To get started with a Amazon SageMaker Ground Truth Plus pilot, do the following\.

1. Under the Ground Truth tab of Amazon SageMaker, choose **Plus**\.

1. On the **SageMaker Ground Truth Plus** page, choose **Request project**\.

1. A page titled **Request a project** opens\. The page includes fields for **General information** and **Project overview**\. Enter the following information

   1. Under **General information**, enter your **First name**, **Last name** and **Business email address**\. An AWS expert uses this information for contacting you to discuss the project after you submit the request\.

   1. Under **Project overview**, enter your **Project name** and **Project description**\. Choose the **Task type** based on your data and use case\. You can also indicate if your data contains personally identifiable information \(PII\)\. 

   1. Create or select an IAM role that grants SageMaker Ground Truth Plus permissions to perform a labeling job by choosing one of the options below\. 

      1. You can **Create an IAM role** that provides access to any S3 bucket you specify\.

      1. You can **Enter a custom IAM role ARN**\.

      1. You can choose an existing role\.

      1. If you use an existing role or a custom IAM role ARN, make sure you have the following IAM role and trust policy\.

         IAM role

         ```
         {
             "Version": "2012-10-17",
             "Statement": [
                 {
                     "Effect": "Allow",
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

         Trust policy

         ```
         {
             "Version": "2012-10-17",
             "Statement": [
                 {
                     "Effect": "Allow",
                     "Principal": {
                         "Service": "sagemaker-ground-truth-plus.amazonaws.com"
                     },
                     "Action": "sts:AssumeRole"
                 }
             ]
         }
         ```

1. Choose **Request a project**\.

Once you create a project, you can find it on the **SageMaker Ground Truth Plus** page, under the Projects section\. The project status should be **Review in\-progress**

**Note**  
You cannot have more than 5 projects with the **Review in progress** status\.