# Supported Regions and Quotas<a name="regions-quotas"></a>

For the AWS Regions supported by Amazon SageMaker and the Amazon Elastic Compute Cloud \(Amazon EC2\) instance types that are available in each Region, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

For a list of the SageMaker service endpoints for each Region and the SageMaker service quotas for each instance type, see [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\.

**Note**  
The following SageMaker features aren't available in the Asia Pacific \(Osaka\) Region:  
Amazon SageMaker Autopilot
Clarify
SageMaker Edge Manager
Ground Truth
Hyperparameter tuning \(HPO\)
Amazon SageMaker Inference Recommender
JumpStart
Amazon SageMaker Model Monitor
Reinforcement learning

Amazon SageMaker Studio is available in all the AWS Regions supported by Amazon SageMaker except the AWS GovCloud \(US\) Regions\. In the supported Regions, Studio is available in the same Availability Zones as notebook instances\.

RStudio on Amazon SageMaker is available in the following AWS Regions supported by Amazon SageMaker\.
+ ap\-northeast\-1
+ ap\-northeast\-2
+ ap\-south\-1
+ ap\-southeast\-1
+ ap\-southeast\-2
+ ca\-central\-1
+ eu\-central\-1
+ eu\-north\-1
+ eu\-west\-1
+ eu\-west\-2
+ eu\-west\-3
+ sa\-east\-1
+ us\-east\-1
+ us\-east\-2
+ us\-west\-1
+ us\-west\-2

Amazon SageMaker Pipelines is available in all the AWS Regions supported by AWS except the AWS GovCloud \(US\) Regions\. SageMaker Projects is available in the AWS regions where CodePipeline is available\. For more information about CodePipeline region availability, see the [AWS Regional Services List](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

For the Availability Zones supported by SageMaker, see [Availability Zones](instance-types-az.md)\.

**About service quotas**  
Depending on your activities and resource usage over time, your SageMaker quotas might be different from the default SageMaker quotas listed on [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html) in the *AWS General Reference*\. If you encounter error messages that you've exceeded your quota and you need to scale up your SageMaker resources, follow the steps in the [Request a service quota increase for SageMaker resources](#service-limit-increase-request-procedure) procedure on this page to request a quota increase from AWS Support\. The SageMaker service team reviews and evaluates requests on case\-by\-case basis\. For additional information, see [How do I resolve the ResourceLimitExceeded error in Amazon SageMaker?](https://aws.amazon.com/premiumsupport/knowledge-center/resourcelimitexceeded-sagemaker/) in the AWS Support Knowledge Center and [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

## Request a service quota increase for SageMaker resources<a name="service-limit-increase-request-procedure"></a>

Follow the steps in this procedure to request a quota \(also known as *limit*\) increase for SageMaker resources\.

**To request a service quota increase**

1. Sign in to the AWS Support console at [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

1. In the left navigation pane, choose **Your support cases**, and then choose **Create case**\.

1. For **Limit type** in the **Case details** section, choose the specific SageMaker component\.

1. For **Request 1** in the **Requests** section, choose **Region**, **Resource Type**, and **Limit** for a resource which you want to increase the quota\. 

1. For **New limit value**, type an integer value for a new quota\.

1. For **Case description**, include the following items:

   1. The error message showing the current quota\. For example, the error message should look like the following:

      ```
      ResourceLimitExceeded: An error occurred (ResourceLimitExceeded) when calling 
      the CreateTrainingJob operation: The account-level service limit 'ml.p3dn.24xlarge 
      for training job usage' is 0 Instances, with current utilization of 0 Instances 
      and a request delta of 1 Instances. 
      Please contact AWS support to request an increase for this limit.
      ```

   1. A brief description of your use case that requires the service quota increase\.

1. If you want to add more quota increase requests for other resources, choose **Add another request** and repeat the previous steps\.

1. Choose **Submit** to finish the request\.