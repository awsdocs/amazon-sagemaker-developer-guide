# Diagnose issues and get support<a name="rstudio-troubleshooting"></a>

 The following sections describe how to diagnose issues with RStudio on Amazon SageMaker\. To get support for RStudio on Amazon SageMaker, contact Amazon SageMaker support\. For help with purchasing an RStudio license or modifying the number of license seats, contact [mailto:sales@rstudio.com](mailto:sales@rstudio.com)\.

## View Metrics and Logs<a name="rstudio-troubleshooting-view"></a>

You can monitor your workflow performance while using RStudio on Amazon SageMaker\. View data logs and information about metrics with the RStudio administrative dashboard or Amazon CloudWatch\. 

### View your RStudio logs from the RStudio administrative dashboard<a name="rstudio-troubleshooting-logs"></a>

 You can view metrics and logs directly from the RStudio administrative dashboard\. 

1.  Log in to your **Amazon SageMaker Domain**\. 

1.  Navigate to the RStudio administrative dashboard following the steps in [RStudio administrative dashboard](rstudio-admin.md)\. 

1.  Select the **Logs** tab\. 

### View your RStudio logs from Amazon CloudWatch Logs<a name="rstudio-troubleshooting-logs"></a>

 Amazon CloudWatch monitors your AWS resources and the applications that you run on AWS in real time\. You can use Amazon CloudWatch to collect and track metrics, which are variables that you can measure for your resources and applications\. To ensure that your RStudio apps have permissions for Amazon CloudWatch, you must include the permissions described in [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. You don’t need to do any setup to gather Amazon CloudWatch Logs\. 

 The following steps show how to view Amazon CloudWatch Logs for your RSession\. 

These logs can be found in the `/aws/sagemaker/studio` log stream from the AWS CloudWatch console\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Select `Logs` from the left side\. From the dropdown menu, select `Log groups`\.

1. On the `Log groups` screen, search for `aws/sagemaker/studio`\. Select the Log group\.

1. On the `aws/sagemaker/studio` `Log group` screen, navigate to the `Log streams` tab\.

1. To find the logs for your Domain, search `Log streams` using the following format:

   ```
   <DomainId>/domain-shared/rstudioserverpro/default
   ```