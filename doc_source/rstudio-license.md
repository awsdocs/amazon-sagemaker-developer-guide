# RStudio license<a name="rstudio-license"></a>

RStudio on Amazon SageMaker is a paid product and requires that each user is appropriately licensed\. Amazon SageMaker does not sell or provide RStudio licenses\. RStudio on Amazon SageMaker requires a separate product license from RStudio\. For customers of RStudio Workbench Enterprise, licenses are issued at no additional cost\. 

To use an RStudio license with Amazon SageMaker, you must first have a valid RStudio license registered with AWS License Manager\. For more information about registering a license with AWS License Manager, see [Seller issued licenses in AWS License Manager](https://docs.aws.amazon.com/license-manager/latest/userguide/seller-issued-licenses.html)\. The following topics show how to acquire and validate an RStudio license\.

 **Get an RStudio license** 

1. If you don't have an RStudio license, you may purchase one at [RStudio Pricing](https://www.rstudio.com/pricing/) or by contacting [mailto:sales@rstudio.com](mailto:sales@rstudio.com)\. When buying or updating an RStudio license, you must provide the AWS Account that will host your Amazon SageMaker Domain\. 

   If you have an existing RStudio license, contact your RStudio Sales representative or [mailto:sales@rstudio.com](mailto:sales@rstudio.com) to add RStudio on Amazon SageMaker to your existing RStudio Workbench Enterprise license, or to convert your RStudio Workbench Standard license\. The RStudio Sales representative will send you the appropriate electronic order form\.

1. RStudio grants a RStudio Workbench license to your AWS Account through AWS License Manager in the US East \(N\. Virginia\) Region\. Although the RStudio license is granted in the US East \(N\. Virginia\) Region, your license can be consumed in any AWS Region that RStudio on Amazon SageMaker is supported in\. You can expect the license grant process to complete within three business days after you share your AWS account ID with RStudio\.

1. When this license is granted, you receive an email from your RStudio Sales representative with instructions to accept your license grant\.

 **Validate your RStudio license to be used with Amazon SageMaker** 

1. Log into the AWS License Manager console in the same region as your Amazon SageMaker Domain\. If you are using AWS License Manager for the first time, you need to grant permission to use AWS License Manager\. 

1.  Select **Create Customer managed license**\. 

1.  Select `I grant AWS License Manager the required permissions` and select **Grant Permissions**\. 

1. Navigate to **Granted Licenses** on the left panel\. 

1. Select the license grant with `RSW-SageMaker` as the `Product name` and select **View**\.

1. From the license detail page, select **Accept & activate license**\. 

 **Get information about your RStudio license usage** 

To share how many users are using your RStudio license, you must get information on this usage from Amazon SageMaker\. You export your usage information, then report it to RStudio PBC\. You can get information about your license usage from AWS License Manager or the RStudio administrative dashboard\. The following shows how to collect and export this information\.  

 **AWS License Manager** 

 AWS License Manager keeps track of your usage for granted licenses and can generate reports of that usage\. For information on how to generate AWS License Manager usage reports, see [License Reporting in License Manager](https://docs.aws.amazon.com/license-manager/latest/userguide/license-reporting.html)\. 

 **RStudio administrative dashboard** 

You can use the RStudio administrative dashboard to see the number of users on the license following the steps in [RStudio administrative dashboard](rstudio-admin.md)\.