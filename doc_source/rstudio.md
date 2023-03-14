# RStudio on Amazon SageMaker<a name="rstudio"></a>

RStudio is an integrated development environment for R, with a console, syntax\-highlighting editor that supports direct code execution, and tools for plotting, history, debugging and workspace management\. Amazon SageMaker supports RStudio as a fully\-managed integrated development environment \(IDE\) integrated with Amazon SageMaker Domain\.

RStudio allows customers to create data science insights using an R environment\. With RStudio integration, you can launch an RStudio environment in the Domain to run your RStudio workflows on SageMaker resources\. For more information about RStudio, see the [RStudio website](https://www.rstudio.com/products/workbench/)\.

**Topics**
+ [Region availability](#rstudio-region)
+ [RStudio components](#rstudio-components)
+ [Differences from RStudio Workbench](#rstudio-differences)
+ [Manage RStudio on Amazon SageMaker](rstudio-manage.md)
+ [Use RStudio on Amazon SageMaker](rstudio-use.md)

SageMaker integrates RStudio through the creation of a RStudioServerPro app\.

 The following are supported by RStudio on SageMaker\. 
+ R developers use the RStudio IDE interface with popular developer tools from the R ecosystem\. Users can launch new RStudio sessions, write R code, install dependencies from RStudio Package Manager, and publish Shiny apps using RStudio Connect\. 
+ R developers can quickly scale underlying compute resources to run large scale data processing and statistical analysis\.  
+ Platform administrators can set up user identities, authorization, networking, storage, and security for their data science teams through AWS IAM Identity Center \(successor to AWS Single Sign\-On\) and AWS Identity and Access Management integration\. This includes connection to private Amazon Virtual Private Cloud \(Amazon VPC\) resources and internet\-free mode with AWS PrivateLink\.
+ Integration with AWS License Manager\. 

 For information on the onboarding steps to create a Domain with RStudio enabled, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

## Region availability<a name="rstudio-region"></a>

The following table gives information about the AWS Regions that RStudio on SageMaker is supported in\.


|  Region name  |  Region  | 
| --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | 
|  US West \(N\. California\)  |  us\-west\-1  | 
|  US West \(Oregon\)  |  us\-west\-2  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  | 
|  Canada \(Central\)  |  ca\-central\-1  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  | 
|  Europe \(Ireland\)  |  eu\-west\-1  | 
|  Europe \(London\)  |  eu\-west\-2  | 
|  Europe \(Paris\)  |  eu\-west\-3  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | 
|  South America \(São Paulo\)  |  sa\-east\-1  | 

## RStudio components<a name="rstudio-components"></a>
+ *RStudioServerPro*: The RStudioServerPro app is a multiuser app that is a shared resource among all user profiles in the Domain\. Once an RStudio app is created in a Domain, the admin can give permissions to users in the Domain\.  
+ *RStudio user*: RStudio users are users within the Domain that are authorized to use the RStudio license\.
+ *RStudio admin*: An RStudio on Amazon SageMaker admin can access the RStudio administrative dashboard\. RStudio on Amazon SageMaker admins differ from "stock" RStudio Workbench admins because they do not have root access to the instance running the RStudioServerPro app and can't modify the RStudio configuration file\.
+ *RStudio Server*: The RStudio Server instance is responsible for serving the RStudio UI to all authorized Users\. This instance is launched on an Amazon SageMaker instance\.
+ *RSession*: An RSession is a browser\-based interface to the RStudio IDE running on an Amazon SageMaker instance\. Users can create and interact with their RStudio projects through the RSession\.
+ *RSessionGateway*: The RSessionGateway app is used to support an RSession\. 
+ *RStudio administrative dashboard*: This dashboard gives information on the RStudio users in the Amazon SageMaker Domain and their sessions\. This dashboard can only be accessed by users that have RStudio admin authorization\.

## Differences from RStudio Workbench<a name="rstudio-differences"></a>

RStudio on Amazon SageMaker has some significant differences from [RStudio Workbench](https://www.rstudio.com/products/workbench/)\.
+ When using RStudio on SageMaker, users don’t have access to the RStudio configuration files\. Amazon SageMaker manages the configuration file and sets defaults\. You can modify the RStudio Connect and RStudio Package Manager URLs when creating your RStudio\-enabled Amazon SageMaker Domain\.
+ Project sharing, realtime collaboration, and Job Launcher are not currently supported when using RStudio on Amazon SageMaker\.
+ When using RStudio on SageMaker, the RStudio IDE runs on Amazon SageMaker instances for on\-demand containerized compute resources\. 
+ RStudio on SageMaker only supports the RStudio IDE and does not support other IDEs supported by an RStudio Workbench installation\.
+ RStudio on SageMaker only supports the RStudio version specified in [Upgrade the RStudio Version ](rstudio-version.md)\.