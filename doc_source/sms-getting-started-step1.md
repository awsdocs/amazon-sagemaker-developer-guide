# Step 1: Before You Begin<a name="sms-getting-started-step1"></a>

Before you begin using the SageMaker console to create a labeling job, you must set up the dataset for use\. Do this:

1. Save two images at publicly available HTTP URLs\. The images are used when creating instructions for completing a labeling task\. The images should have an aspect ratio of around 2:1\. For this exercise, the content of the images is not important\.

1. Create an Amazon S3 bucket to hold the input and output files\. The bucket must be in the same Region where you are running Ground Truth\. Make a note of the bucket name because you use it during step 2\.

1. Place 5–10 PNG images in the bucket\.

1. Create a manifest file for the dataset and store it in the S3 bucket\. Use these steps: 

   1. Using a text editor, create a new text file\.

   1. Add a line similar to the following for each image file in your dataset:

      ```
      {"source-ref": "s3://bucket/path/imageFile.png"}
      ```

      Add one line for each PNG file in your S3 bucket\.

   1. Save the file in the S3 bucket containing your source files\. Record the name because you use it in step 2\.
**Note**  
It is not necessary to store the manifest file in the same bucket as the source file\. You use the same bucket in this exercise because it is easier\.

   For more information, see [Input Data](sms-data-input.md)\.

Assign the following permissions policy to the user that is creating the labeling job:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "sagemakergroundtruth",
            "Effect": "Allow",
            "Action": [
                "cognito-idp:CreateGroup",
                "cognito-idp:CreateUserPool",
                "cognito-idp:CreateUserPoolDomain",
                "cognito-idp:AdminCreateUser",
                "cognito-idp:CreateUserPoolClient",
                "cognito-idp:AdminAddUserToGroup",
                "cognito-idp:DescribeUserPoolClient",
                "cognito-idp:DescribeUserPool",
                "cognito-idp:UpdateUserPool"
            ],
            "Resource": "*"
        }
    ]
}
```

## Next<a name="step1-next"></a>

[Step 2: Create a Labeling Job](sms-getting-started-step2.md)