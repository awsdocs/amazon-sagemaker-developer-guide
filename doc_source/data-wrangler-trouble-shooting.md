# Troubleshoot<a name="data-wrangler-trouble-shooting"></a>

If an issue arises when using Amazon SageMaker Data Wrangler, we recommend you do the following:
+ If an error message is provided, read the message and resolve the issue it reports if possible\.
+ Make sure the IAM role of your Studio user has the required permissions to perform the action\. For more information, see [Security and Permissions](data-wrangler-security.md)\.
+ If the issue occurs when you are trying to import from another AWS service, such as Amazon Redshift or Athena, make sure that you have configured the necessary permissions and resources to perform the data import\. For more information, see [Import](data-wrangler-import.md)\.
+ If you're still having issues, choose **Get help** at the top right of your screen to reach out to the Data Wrangler team\. For more information, see the following images\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/get-help/get-help.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/get-help/get-help-forms.png)

As a last resort, you can try restarting the kernel on which Data Wrangler is running\. 

1. Save and exit the \.flow file for which you want to restart the kernel\. 

1. Select the ****Running Terminals and Kernels**** icon, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/stop-kernel-option.png)

1. Select the **Stop** icon to the right of the \.flow file for which you want to terminate the kernel, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/stop-kernel.png)

1. Refresh the browser\. 

1. Reopen the \.flow file on which you were working\. 