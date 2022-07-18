# RStudio license<a name="rstudio-license"></a>

RStudio on Amazon SageMaker is a paid product and requires that each user is appropriately licensed\. Licenses for RStudio on Amazon SageMaker may be obtained from RStudio PBC directly, or by purchasing a subscription to RStudio Workbench on AWS Marketplace\. For existing customers of RStudio Workbench Enterprise, licenses are issued at no additional cost\. 

To use an RStudio license with Amazon SageMaker, you must first have a valid RStudio license registered with AWS License Manager\. Subscriptions to RStudio Workbench on AWS Marketplace automatically trigger license creation with AWS License Manager\. For licenses purchased directly through Rstudio PBC, a licenses grant for your AWS Account must be created\. Contact RStudio for direct license purchases or to enable existing licenses in AWS License Manager\. For more information about registering a license with AWS License Manager, see [Seller issued licenses in AWS License Manager](https://docs.aws.amazon.com/license-manager/latest/userguide/seller-issued-licenses.html)\. 

The following topics show how to acquire and validate a license granted by RStudio PBC\.

 **Get an RStudio license** 

1. If you don't have an RStudio license, you may purchase one at [RStudio Pricing](https://www.rstudio.com/pricing/) or by contacting [sales@rstudio\.com](mailto:sales@rstudio.com)\. When buying or updating an RStudio license, you must provide the AWS Account that will host your Amazon SageMaker Domain\. 

   If you have an existing RStudio license, contact your RStudio Sales representative or [sales@rstudio\.com](mailto:sales@rstudio.com) to add RStudio on Amazon SageMaker to your existing RStudio Workbench Enterprise license, or to convert your RStudio Workbench Standard license\. The RStudio Sales representative will send you the appropriate electronic order form\.

1. RStudio grants a RStudio Workbench license to your AWS Account through AWS License Manager in the US East \(N\. Virginia\) Region\. Although the RStudio license is granted in the US East \(N\. Virginia\) Region, your license can be consumed in any AWS Region that RStudio on Amazon SageMaker is supported in\. You can expect the license grant process to complete within three business days after you share your AWS account ID with RStudio\.

1. When this license is granted, you receive an email from your RStudio Sales representative with instructions to accept your license grant\.

 **Validate your RStudio license to be used with Amazon SageMaker** 

1. Log into the AWS License Manager console in the same region as your Amazon SageMaker Domain\. If you are using AWS License Manager for the first time, you need to grant permission to use AWS License Manager\. 

1.  Select **Create Customer managed license**\. 

1.  Select `I grant AWS License Manager the required permissions` and select **Grant Permissions**\. 

1. Navigate to **Granted Licenses** on the left panel\. 

1. Select the license grant with `RSW-SageMaker` as the `Product name` and select **View**\.

1. From the license detail page, select **Accept & activate license**\. 

 **RStudio administrative dashboard** 

You can use the RStudio administrative dashboard to see the number of users on the license following the steps in [RStudio administrative dashboard](rstudio-admin.md)\.