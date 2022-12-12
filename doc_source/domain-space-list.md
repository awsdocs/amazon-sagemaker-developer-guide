# List and Describe shared spaces<a name="domain-space-list"></a>

 This guide shows how to access a list of shared spaces in an Amazon SageMaker Domain with the Amazon SageMaker console or the AWS CLI\. It also shows how to view details of a shared space from the AWS CLI\. 

**Topics**
+ [List shared spaces](#domain-space-list-spaces)
+ [View shared space details](#domain-space-describe)

## List shared spaces<a name="domain-space-list-spaces"></a>

 The following topic describes how to view a list of shared spaces within a Domain from the SageMaker console or the AWS CLI\. 

### List shared spaces from the console<a name="domain-space-list-console"></a>

 Complete the following procedure to view a list of the shared spaces in a Domain from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to view the list of shared spaces for\. 

1.  On the **Domain details** page, choose the **Space management** tab\. 

### List shared spaces from the AWS CLI<a name="domain-space-list-cli"></a>

 To list the shared spaces in a Domain from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws --region region \
sagemaker list-spaces \
--domain-id domain-id
```

## View shared space details<a name="domain-space-describe"></a>

 The following section describes how to view shared space details from the SageMaker console or the AWS CLI\. 

### View shared space details from the console<a name="domain-space-describe-console"></a>

 You can view the details of a shared space from the SageMaker console using the following procedure\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to view the list of shared spaces for\. 

1.  On the **Domain details** page, choose the **Space management** tab\. 

1.  Select the name of the space to open a new page that lists details about the shared space\. 

### View shared space details from the AWS CLI<a name="domain-space-describe-cli"></a>

To view the details of a shared space from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws --region region \
sagemaker describe-space \
--domain-id domain-id \
--space-name space-name
```