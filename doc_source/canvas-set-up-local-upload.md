# Grant Your Users Permissions to Upload Local Files<a name="canvas-set-up-local-upload"></a>

If your users are uploading files from their local machines to SageMaker Canvas, you must attach a CORS configuration to the Amazon S3 bucket that they're using\. When one of your users first accesses SageMaker Canvas, SageMaker creates an Amazon S3 bucket with a name that uses the following pattern: `sagemaker-{Region}-{account-ID}`\. SageMaker Canvas adds your users' data to the bucket whenever they upload a file\.

To grant users permissions to upload local files to the bucket, you can attach a CORS configuration to it using either of the following procedures\. You can use the first method when setting up your Domain or editing the existing Domain settings, where you opt in to allow SageMaker to attach the CORS configuration to the default bucket for you\. The second method is the manual method, where you can attach the CORS configuration to the bucket yourself\.

## Domain setup method<a name="canvas-set-up-local-upload-domain"></a>

To grant your users permissions to upload local files, you can choose **Enable Canvas permissions** when setting up your Domain\. This attaches a Cross\-Origin Resource Sharing \(CORS\) configuration to the SageMaker Amazon S3 bucket created for your account and grants all users in the Domain permission to upload local files into SageMaker Canvas\. By default, the permissions option is turned on when you set up a Domain, but you can turn off this option if you don’t want to grant your users permission to upload files\.

**Note**  
If you have an existing CORS configuration on the SageMaker Amazon S3 bucket, turning on **Enable Canvas permissions** overwrites the existing configuration with the new configuration\.

The following procedure shows how you can turn on this option when doing a **Quick setup** for your Domain in the console\.

1. In the **User profile** section, enter a **Name** for the user\.

1. Select an **Execution role** for the user\.

1. Turn on **Enable SageMaker Canvas permissions**\. \(By default, this option is turned on\.\)

1. Finish setting up the Domain\.

If you are doing a **Standard setup** for your Domain, then use the following procedure for the **Canvas settings** section to turn on local file upload\.

1. For **Enable and configure Canvas permissions**, select **Local file upload**\. \(It's already checked by default\.\)

1. Choose **Next**\.

1. Finish setting up the Domain\.

Your users can now upload local files into their SageMaker Canvas application\.

You can also turn on or turn off local upload permissions for an existing Domain by using the following procedure\.

1. Go to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. In the **Control Panel**, choose **Edit domain settings**\.

1. Go to **Canvas settings**\.

1. Select or deselect **Local file upload**\.

1. Finish any other modifications you want to make to the Domain, and then choose **Submit** to submit your changes\.

## Amazon S3 bucket method<a name="canvas-set-up-local-upload-s3"></a>

If you want to manually attach the CORS configuration to the SageMaker Amazon S3 bucket, use the following procedure\.

1. Sign in to [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket with the name that uses the following pattern: `sagemaker-{region}-{account-ID}`\.

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