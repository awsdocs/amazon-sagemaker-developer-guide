# Give Your Users Access to Upload Local Files<a name="canvas-set-up-local-upload"></a>

If your users are uploading files from their local machines, attach a CORS policy to the Amazon S3 bucket that they're using\. When one of your users first accesses Amazon SageMaker Canvas, Amazon SageMaker creates an Amazon S3 bucket with a name that uses the following pattern: `sagemaker-AWS-Region-AWS-account-id`\. SageMaker Canvas can add your users' data to the bucket whenever they upload a file\. To give them access to upload local files to the bucket, you attach a CORS policy to it using the following procedure\.

1. Sign in to [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket with the name that uses the following pattern: `sagemaker-Region-your-account-id`\.

1. Choose **Permissions**\.

1. Navigate to **Cross\-origins resource sharing \(CORS\)**\.

1. Choose **Edit**\.

1. Add the following CORS policy:

   ```
   [
       {
           "AllowedHeaders": [
               "*"
           ],
           "AllowedMethods": [
               "POST"
           ],
           "AllowedOrigins": [
               "*"
           ],
           "ExposeHeaders": []
       }
   ]
   ```

1. Choose **Save changes**\.

In the preceding procedure, the CORS policy must have `"POST"` listed under `AllowedMethods`\.

After you've gone through the procedure, you should have:
+ An IAM role assigned to each of your users\.
+ Amazon SageMaker Studio runtime permissions for each of your users\. SageMaker Canvas uses Studio to run the commands from your users\.
+ If the users are uploading files from their local machines, a CORS policy attached to their Amazon S3 bucket\.

If your users still can't upload the local files after you update the CORS policy, the browser might be caching the CORS settings from a previous upload attempt\. If they're running into issues, instruct them to clear their browser cache and try again\.