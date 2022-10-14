# Give Your Users Permissions to Perform Time Series Forecasting<a name="canvas-set-up-forecast"></a>

In order to perform time series forecasts in Amazon SageMaker Canvas, your users must have the necessary permissions\. The preferred method to give your users these permissions is to enable the time series forecasting option when setting up the Amazon SageMaker Domain, or when editing the settings for a specific user profile\. You can also use the manual method of attaching a policy and trust relationship for Amazon Forecast to the AWS Identity and Access Management \(IAM\) role\.

## Domain setup method<a name="canvas-set-up-forecast-domain"></a>

SageMaker provides you with the option to give time series forecasting permissions to users through the Domain settings\. You can toggle the permissions for all of the users in your Domain, and SageMaker manages attaching the required IAM policy and trust relationship for you\.

If you are setting up your Amazon SageMaker Domain for the first time and want to turn on time series forecasting permissions for all users in the Domain, then use the following procedures\.

------
#### [ Quick setup ]

Use the following procedure to turn on SageMaker Canvas time series forecasting permissions when doing a Quick setup for your Domain\.

1. In the Amazon SageMaker Domain Quick setup, fill out the **Name** and **Default execution role **fields in the **User profile** section\.

1. Leave the **Enable SageMaker Canvas permissions** option turned on \(it is turned on by default\)\.

1. Choose **Submit** to finish setting up your Domain\.

------
#### [ Standard setup ]

Use the following procedure to turn on SageMaker Canvas time series forecasting permissions when doing a Standard setup for your Domain\.

1. In the Amazon SageMaker Domain Standard setup, fill out the **General settings**, **Studio settings**, and **RStudio settings** pages\.

1. Choose the **Canvas settings** page\.

1. For the **Canvas base permissions configuration**, leave the **Enable Canvas base permissions** option turned on \(it is turned on by default\)\. These permissions are required in order to turn on time series forecasting permissions\.

1. For the **Time series forecasting configuration**, leave the **Enable time series forecasting** option turned on \(it is turned on by default\)\.

1. Select **Create and use a new execution role**, or select **Use an existing execution role** if you already have an IAM role with the required Amazon Forecast permissions attached \(for more information, see the [IAM role setup method](#canvas-set-up-forecast-iam)\)\.

1. Finish making any other changes to your Domain setup, and then choose **Submit**\.

------

Your users should now have the necessary permissions to perform time series forecasting in SageMaker Canvas\.

## User setup method<a name="canvas-set-up-forecast-user"></a>

You can configure time series forecasting permissions for individual users in an existing Domain\. The user profile settings override the general Domain settings, so you can give permissions to specific users without giving permissions to all of your users\. To give time series forecasting permissions to a specific user that doesn't already have permissions, use the following procedure\.

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the **Control Panel**, in the **Users** panel, select the name of the user whose permissions you want to edit\.

1. On the **User Details** page, choose **Edit**\.

1. Choose the **Canvas settings** page\.

1. Turn on **Enable Canvas base permissions**\. These permissions are required in order to turn on time series forecasting permissions\.

1. Turn on the **Enable time series forecasting** option\.

1. If you want to use a different execution role for the user than the role specified in the Domain, select **Create and use a new execution role**, or **Use an existing execution role** if you already have an IAM role ready to use\.
**Note**  
If you want to use an existing IAM role, make sure that it has the IAM policy `AmazonSageMakerCanvasForecastAccess` attached and has a trust relationship that establishes Amazon Forecast as a service principal\. For more information, see the section [IAM role setup method](#canvas-set-up-forecast-iam)\.

1. The **Canvas settings** page should look like the following screenshot\. Finish making any other changes to your user profile, and then choose **Submit** to save your changes\.  
![\[Screenshot of the SageMaker Canvas settings page while editing the Domain settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-domain-time-series-config.png)

Your user should now have permission to do time series forecasting in SageMaker Canvas\.

You can also remove permissions from a user by using the preceding procedure and turning off the **Enable time series forecasting** option\.

## IAM role setup method<a name="canvas-set-up-forecast-iam"></a>

You can manually give your users permissions to perform time series forecasting in Amazon SageMaker Canvas by adding additional permissions to the AWS Identity and Access Management \(IAM\) role specified for the user’s profile\. The IAM role must have a trust relationship with Amazon Forecast and an attached policy that gives permissions to Forecast\.

The following section shows you how to create the trust relationship and attach the `[AmazonSageMakerCanvasForecastAccess](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol-canvas.html#security-iam-awsmanpol-AmazonSageMakerCanvasForecastAccess)` managed policy to your IAM role, which grants the minimum permissions necessary for time series forecasting to work in SageMaker Canvas\.

To configure an IAM role with the manual method, use the following procedure\.

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Control Panel**\.

1. From the list of **Users**, select the profile of the user you to whom want to give time series forecasting permissions\.

1. Under **Details**, copy or make a note of the name of the user's **Execution role**\. The name of the IAM role should be similar to the following: `AmazonSageMaker-ExecutionRole-111122223333444`\.  
![\[Screenshot of the user's profile in the SageMaker console.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-find-role.png)

1. Once you have the name of the user's IAM role, go to the [IAM console](https://console.aws.amazon.com/iamv2)\.

1. Choose **Roles**\.

1. Search for the user's IAM role by name from the list of roles and select it\.

1. Under **Permissions**, choose **Add permissions**\.

1. Choose **Attach policies**\.  
![\[Screenshot of the Attach policies button under Add permissions.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-add-permissions.png)

1. Search for the `[AmazonSageMakerCanvasForecastAccess](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol-canvas.html#security-iam-awsmanpol-AmazonSageMakerCanvasForecastAccess)` managed policy and select it\. Choose **Attach policies** to attach the policy to the role\.

   After attaching the policy, the role's **Permissions** section should now include `AmazonSageMakerCanvasForecastAccess`\.

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

1. After editing the trust policy, choose **Update policy**\.

You should now have an IAM role that has the policy `[AmazonSageMakerCanvasForecastAccess](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol-canvas.html#security-iam-awsmanpol-AmazonSageMakerCanvasForecastAccess)` attached to it and a trust relationship established with Amazon Forecast, giving users permission to perform time series forecasting in SageMaker Canvas\. For information about AWS managed policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html)\.