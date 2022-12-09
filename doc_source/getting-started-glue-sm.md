# Get Started with AWS Glue Interactive Sessions<a name="getting-started-glue-sm"></a>

This guide will show you how set up the necessary permissions, launch your first Glue interactive session in Studio, and manage your environment with Jupyter magics\.

## Permissions for Glue Interactive Sessions in SageMaker Studio<a name="glue-sm-iam"></a>

This guide will show you how to obtain the required permissions to use Glue Interactive Sessions in Studio\. You will attach two managed policies, then create and attach a custom policy, and lastly, modify the trust relationship\.

**To add the managed policies to your execution role**

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. Select **Roles** in the left\-side panel\.

1. Find the Studio execution role that you will use, and choose the role name to go to the role summary page\. 

1. Under the **Permissions** tab, select **Attach policies** from the **Add Permissions** dropdown menu\.

1. Select the checkbox next to the managed policy `AwsGlueSessionUserRestrictedServiceRole`\.

1. Choose **Attach policies**\. 

   The summary page shows your newly\-added managed policies\.

   

**To create the custom policy and attach it to your execution role**

1. Select **Create inline policy** in the **Add Permissions** dropdown menu\.

1. Select the **JSON** tab\.

1. Copy and paste in the following policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "unique_statement_id",
   
               "Effect": "Allow",
               "Action": [
   		"iam:GetRole",
                   "iam:PassRole",
                   "sts:GetCallerIdentity"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. Enter a **Name** and choose **Create policy**\. 

   The summary page shows your newly\-added custom policy\.

   

**To modify the trust relationship**

1. Select the **Trust relationships** tab\.

1. Chose **Edit trust policy**\.

1. Copy and paste in the following policy\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": [
                       "glue.amazonaws.com",
                       "sagemaker.amazonaws.com"
                   ]
               },
               "Action": "sts:AssumeRole"
           }
       ]
   }
   ```

1. Choose **Update policy**\.

You can add additional roles and policies if you need to access other AWS resources\. For a description of the additional roles and policies you can include, see [Interactive sessions with IAM](https://docs.aws.amazon.com/glue/latest/dg/glue-is-security.html) in the AWS Glue documentation\.

## Launch your Glue interactive session on SageMaker Studio<a name="glue-sm-launch"></a>

After you create the roles, policies, and SageMaker domain, you can launch your Glue interactive session in SageMaker Studio\.

**To launch Glue in SageMaker Studio**

1. Create a SageMaker domain\. For instructions on how to create a new domain, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. Sign in to the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Select **Control Panel** in the left\-side panel\.

1. In the **Launch App** dropdown menu next to the user name, select **Studio**\.

1. In the Jupyter view, choose **File**, then **New**, then **Notebook**\.

1. In the **Image** dropdown menu, select **SparkAnalytics 1\.0** or **SparkAnalytics 2\.0**\. In the **kernel** dropdown menu, select **Glue Spark** or **Glue Python \[PySpark and Ray\]**\. Choose **Select**\.

1. \(optional\) Use Jupyter magics to customize your environment\. For more information about Jupyter magics, see [Configure your Glue interactive session in SageMaker Studio](#glue-sm-magics)\.

1. Start writing your Spark data processing scripts\.

## Configure your Glue interactive session in SageMaker Studio<a name="glue-sm-magics"></a>

You can use Jupyter magics in your AWS Glue interactive session to modify your session and configuration parameters\. Magics are short commands prefixed with `%` at the start of Jupyter cells that provide a quick and easy way to help you control your environment\. In your Glue interactive session, the following magics are set for you by default:


| Magic | Default value | 
| --- | --- | 
| %glue\_version |  3\.0  | 
| %iam\_policy |  *execution role attached to your SageMaker domain*  | 
| %region |  your region  | 

You can use magics to further customize your environment\. For example, if you want to change the number of workers allocated to your job from the default five to 10, you can specify `%number_of_workers 10`\. If you want to configure your session to stop after 10 minutes of idle time instead of the default 2880, you can specify `%idle_timeout 10`\.

All of the Jupyter magics currently available in AWS Glue are also available in SageMaker Studio\. For the complete list of AWS Glue magics available, see [Configuring AWS Glue interactive sessions for Jupyter and AWS Glue Studio notebooks](https://docs.aws.amazon.com/glue/latest/dg/interactive-sessions-magics.html)\.