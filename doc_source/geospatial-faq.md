# SageMaker geospatial capabilities FAQ<a name="geospatial-faq"></a>


****  

|  | 
| --- |
| Amazon SageMaker geospatial is in preview release for Amazon SageMaker and is subject to change\. We do not recommend using this feature in production environments\. | 

Use the following FAQ items to find answers to commonly asked questions about SageMaker geospatial capabilities\.

1. **What regions are Amazon SageMaker geospatial capabilities available in?**

   Currently, SageMaker geospatial capabilities are only supported in the US West \(Oregon\) Region for public preview\. To view SageMaker geospatial, choose the name of the currently displayed Region in the navigation bar of the console\. Then choose the US West \(Oregon\) Region\.

1. **How can I setup a user role and an execution role to get started with SageMaker geospatial capabilities?**

   As a managed service, SageMaker geospatial capabilities perform operations on your behalf on the AWS hardware managed by SageMaker\. It can only perform the operations that the user permits\. To work with SageMaker geospatial capabilities, you need to setup a user role and an execution role\. See [SageMaker geospatial capabilities roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-geospatial-roles.html) to learn more\.

1. **Can I use SageMaker geospatial capabilities through my VPC environment?**

    No, currently SageMaker geospatial capabilities only support a public internet environment\.

1. **Why can't I see the SageMaker geospatial UI link when I navigate to Amazon SageMaker Studio?**

   Verify that you are launching Amazon SageMaker Studio in the US West \(Oregon\) Region and that you are not in an VPC only environment or in a shared space environment\.

1. **How to create a notebook job in Studio?**

   See [Schedule a notebook job ](https://docs.aws.amazon.com/sagemaker/latest/dg/create-notebook-auto-run.html) to learn how to create and manage your notebook jobs\. Make sure you are using the latest JupyterLab version\.

1. **What bands supported for various raster data collections?**

   Use the `GetRasterDataCollection` API response and refer to the `ImageSourceBands` field to find the bands supported for that particular data collection\.

1. **Can I use SageMaker geospatial capabilities if my browser does not have internet connection?**

   You cannot access the list of EOJs, VEJs as well as map visualization from the UI if your browser does not have internet connection\.