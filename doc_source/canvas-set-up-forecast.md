# Give your users permissions to perform time series forecasting<a name="canvas-set-up-forecast"></a>

To give your users permissions to perform time series analyses in Amazon SageMaker Canvas, you must add additional permissions to the AWS IAM role you chose when setting up the user's profile\.

To give your users the IAM permissions required to do time series forecasting, do the following\.

1. Sign in to the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/home) and choose **SageMaker Domain**\.

1. From the list of **Users**, select the profile of the user you to whom want to give time series forecasting permissions\.

1. Under **Details**, copy or make a note of the name of the user's **Execution role**\. The name of the IAM role should be similar to the following: `AmazonSageMaker-ExecutionRole-111122223333444`\.  
![\[Screenshot of the user's profile in the SageMaker console.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-find-role.png)

1. Once you have the name of the user's IAM role, go to the [IAM console](https://console.aws.amazon.com/iamv2)\.

1. Choose **Roles**\.

1. Search for the user's IAM role by name from the list of roles and select it\.  
![\[Screenshot of the search box in the IAM console.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-search-iam-role.png)

1. Under **Permissions**, choose **Add permissions**\.

1. Choose **Attach policies**\.  
![\[Screenshot of the Attach policies button under Add permissions.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-add-permissions.png)

1. Search for the `AmazonForecastFullAccess` managed policy and select it\. Choose **Attach policies** to attach the policy to the role\.

   After attaching the policy, the role's **Permissions** section should now include `AmazonForecastFullAccess`, as shown in the following screenshot\.  
![\[Screenshot of the AmazonForecastFullAccess managed policy listed in the role's Permissions section.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-added-policy.png)

1. Return to the IAM role's page, and under **Trust relationships**, choose **Edit trust policy**\.

1. In the **Edit trust policy** editor, update the trust policy to add Forecast as a service principal\. The policy should look like the following example\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": [
               "sagemaker.amazonaws.com",
               "forecast.amazonaws.com"
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. After editing the trust policy, choose **Update policy**\. The trust relationship should now look like the following screenshot\.  
![\[Screenshot of the IAM role's trust relationship, which includes Forecast and SageMaker.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-added-trust.png)

The user should now have permission to perform time series forecasting in SageMaker Canvas\. For information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html)\.