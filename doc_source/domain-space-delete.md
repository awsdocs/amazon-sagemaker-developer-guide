# Delete a shared space<a name="domain-space-delete"></a>

 The following topic shows how to delete a shared space from the Amazon SageMaker console or AWS CLI\. A shared space can only be deleted if it has no running applications\. 

**Topics**
+ [Console](#domain-space-delete-console)
+ [AWS CLI](#domain-space-delete-cli)

## Console<a name="domain-space-delete-console"></a>

 Complete the following procedure to delete a shared space in the Amazon SageMaker Domain from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to create a shared space for\. 

1.  On the **Domain details** page, choose the **Space management** tab\. 

1.  Select the shared space that you want to delete\. The shared space must not contain any non\-failed apps\. 

1.  Choose **Delete**\. This opens a new window\. 

1.  Choose **Yes, delete space**\. 

1.  Enter *delete* in the field\. 

1.  Choose **Delete space**\. 

## AWS CLI<a name="domain-space-delete-cli"></a>

To delete a shared space from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws --region region \
sagemaker delete-space \
--domain-id domain-id \
--space-name space-name
```