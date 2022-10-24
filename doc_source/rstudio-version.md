# Upgrade the RStudio Version<a name="rstudio-version"></a>

In this guide, you'll get information about the latest version update for RStudio on SageMaker\. You must update your version to provide continued functionality\. To upgrade to the latest RStudio version on Amazon SageMaker, complete the steps in [Shut down and restart RStudio](rstudio-shutdown.md)\.

## Latest version updates<a name="rstudio-version-latest"></a>

The updated RStudio version is `2022.02.2-485.pro2`\. This version supports:
+ Enhanced R help system, introduced with R 4\.2\.0\.
+ Added editor support for the R pipe\-bind placeholder \(`_`\)\.
+ Encryption between RStudioServerPro and RSession applications\. This update requires upgrading existing RStudio\-enabled SageMaker domains\. For more information, see [Upgrade Scenarios](#rstudio-version-scenarios)\.

All newly created SageMaker domains with RStudio and RSession support this new version\. To use this new version with existing SageMaker domains with RStudio, you must relaunch your RStudioServerPro application\. For more information about the changes in this release, see the [RStudio Release Notes](https://www.rstudio.com/products/rstudio/release-notes/#rstudio-2022-02-2-prairie-trillium)\. 

## Upgrade Scenarios<a name="rstudio-version-scenarios"></a>

 All new RStudio applications are created using the `2022.02.2-485.pro2` release\. The RStudioServerPro application is deployed when the domain is created, and it persists unless it's deleted\. If you have an existing RStudio\-enabled domain, you must upgrade the RStudioServerPro application to support end\-to\-end encryption with new RSessions\. If you create a new domain, you don't need to upgrade\. 

 Your upgrade status is one of the following: 
+  **If you create a new domain with RStudio Enabled**: RStudio applications for newly created domains are created using the `2022.02.2-485.pro2` release and support end\-to\-end encryption\. No further action is required\. 
+  **You have a pre\-existing SageMaker domain with RStudio and update the RStudioServerPro App: **This enables end\-to\-end encryption and requires no further changes for new RSessions\. You must delete and re\-create your existing RSession applications\. 
+  **You have a pre\-existing SageMaker domain with RStudio and do not update the RStudioServerPro App: **If you don’t update your application, there is a version mismatch with all new RSessions\. There may be functionality issues because of the version mismatch\. Traffic encryption between RStudioServerPro and RSession is also not available\. We recommend that you update your RStudioServerPro application to the new version\.

## Changes to BYOI Images<a name="rstudio-version-byoi"></a>

 If you are using a BYOI image with RStudio, you must upgrade your custom images to use the `2022.02.2-485.pro2` release and redeploy your existing RSessions\. If you attempt to load a non\-compatible image in an RSession of a domain using the `2022.02.2-485.pro2` version, the RSession fails because it cannot parse parameters that it receives\. Your RSession will not function properly and will not support end\-to\-end encryption\. You must update all of the deployed custom images in your existing RStudioServerPro app\. 