# Amazon SageMaker Autopilot quotas<a name="autopilot-quotas"></a>

There are quotas that limit the resources available to you when using Amazon SageMaker Autopilot\. Some of these limits are increasable and some are not\. 

**Note**  
The resource quotas documented in the following sections are valid for versions of Amazon SageMaker Studio 3\.22\.2 and higher\. For information on updating your version of SageMaker Studio, see [Shut Down and Update SageMaker Studio and Studio Apps](studio-tasks-update.md)\.

**Topics**
+ [Quotas that you can increase](#autopilot-quotas-limits-increasable)
+ [Resource quotas](#autopilot-quotas-resource-limits)

## Quotas that you can increase<a name="autopilot-quotas-limits-increasable"></a>

There are default limits for the size of the input datasets: file size of a single Parquet file \(\*\), the target dataset size subsampling \(\*\*\), and the number of concurrent jobs you can run with Amazon SageMaker Autopilot for each AWS account per AWS Region\. 


**Resource limits**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-quotas.html)

**Note**  
\*This 2 GB size limit is for a single compressed Parquet file\. You can provide a Parquet dataset that includes multiple compressed Parquet files\. After the files are decompressed, they may each expand to a larger size\.  
\*\*Autopilot automatically subsamples input datasets that are larger than the target dataset size while accounting for class imbalance and preserving rare class labels\.

You can increase these limits by contacting AWS Support\.

**To request a quota increase:**

1. Open the [AWS Support Center](https://console.aws.amazon.com/support/home#/) page, sign in if necessary, and then choose **Create case**\. 

1. On the **Create case** page, choose **Service limit increase**\.

1. In the **Case details** panel, select **SageMaker AutoML** for the **Limit Type**\.

1. On the **Requests** panel for **Request 1**, select the **Region**, the resource **Limit** to increase, and the **New Limit value** that you are requesting\. If you have additional requests for quota increases, select **Add another request**\.   
![\[The Create case page in the AWS Support Center to request a quota increase in Autopilot.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/autopilot/automl-quotas-service-limit-increase-request.PNG)

1. Provide your preferred **Contact options** and choose **Submit**\. 

## Resource quotas<a name="autopilot-quotas-resource-limits"></a>

The following table contains the runtime resource limits for an Amazon SageMaker Autopilot job in an AWS Region\.


**Resource limits per Autopilot job**  

| Resource | Limit per Autopilot job | 
| --- | --- | 
| Maximum runtime for an Autopilot job | 30 days | 