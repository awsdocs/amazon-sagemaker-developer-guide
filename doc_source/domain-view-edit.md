# View and Edit Domains<a name="domain-view-edit"></a>

 This topic shows how to view a list of your Amazon SageMaker Domains, view the details of a Domain, and edit Domain settings from the Amazon SageMaker console or AWS Command Line Interface \(AWS CLI\)\. 

**Topics**
+ [View Domains](#domain-view-domains)
+ [Edit Domain settings](#domain-edit-domains)

## View Domains<a name="domain-view-domains"></a>

 The following section shows how to view a list of your Domains, and details of an individual Domain from the SageMaker console or the AWS CLI\. 

### Console<a name="domain-view-domains-console"></a>

 The console's Domain overview page gives information about the structure of a Domain, and it provides a list of your Domains\. The page's Domain structure diagram describes Domain components and how they interact with each other\. 

The following procedure shows how to view a list of your Domains from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose **Domains**\. 

 To view the details of the Domain, complete the following procedure\. This page gives information about the general settings for the Domain, including the name, Domain ID, execution role used to create the Domain, and the authentication method of the Domain\.  

1.  From the list of Domains, select the Domain that you want to open the **Domain settings** page for\. 

1.  On the **Domain details** page, choose the **Domain settings** tab\. 

### AWS CLI<a name="domain-view-domains-cli"></a>

 Run the following command from the terminal of your local machine to view a list of Domains from the AWS CLI\. 

```
aws sagemaker list-domains --region region
```

## Edit Domain settings<a name="domain-edit-domains"></a>

 You can edit the settings of a Domain from the SageMaker console or the AWS CLI\. The following considerations apply when updating the settings of a Domain\.
+ If `DefaultUserSettings` and `DefaultSpaceSettings` are set, they cannot be unset\.
+ `DefaultUserSettings.ExecutionRole` can only be updated if there are no applications running in any user profile within the Domain\. This value cannot be unset\.
+ `DefaultSpaceSettings.ExecutionRole` can only be updated if there are no applications running in any of shared spaces within the Domain\. This value cannot be unset\.
+ If the Domain was created in **VPC only** mode, SageMaker automatically applies updates to the security group settings defined for the Domain to all shared spaces created in the Domain\.
+ `DomainId` cannot be updated\.

 The following section shows how to edit Domain settings from the SageMaker console or the AWS CLI\. 

### Console<a name="domain-edit-domains-console"></a>

 You can edit the Domain from the SageMaker console using the following procedure\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to open the **Domain settings** page for\. 

1. On the **Domain details** page, choose the **Domain settings** tab\. 

1.  Choose **Edit**\. 

### AWS CLI<a name="domain-edit-domains-cli"></a>

 Run the following command from the terminal of your local machine to update a Domain from the AWS CLI\. For more information about the structure of `default-user-settings`, see [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html#API_CreateDomain_RequestSyntax)\.

```
aws sagemaker update-domain \
--domain-id domain-id \
--default-user-settings default-user-settings \
--default-space-settings default-space-settings \
--domain-settings-for-update settings-for-update \
--region region
```