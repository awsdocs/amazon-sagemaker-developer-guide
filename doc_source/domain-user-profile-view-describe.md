# View User Profiles and User Profile Details<a name="domain-user-profile-view-describe"></a>

 This topic shows how to view a list of user profiles in an Amazon SageMaker Domain, and view details for a user profile from the SageMaker console or the AWS Command Line Interface \(AWS CLI\)\. 

**Topics**
+ [View user profiles](#domain-user-profile-view)
+ [View user profile details](#domain-user-profile-describe)

## View user profiles<a name="domain-user-profile-view"></a>

 The following section describes how to view a list of user profiles in a Domain from the SageMaker console or the AWS CLI\. 

### View user profiles from the console<a name="domain-user-profile-view-console"></a>

 Complete the following procedure to view a list of user profiles in the Domain from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to view a list of user profiles for\. 

1. On the **Domain details** page, choose the **User profiles** tab\. 

### View user profiles from the AWS CLI<a name="domain-user-profile-view-cli"></a>

To view the user profiles in a Domain from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws sagemaker list-user-profiles \
--region region \
--domain-id domain-id
```

## View user profile details<a name="domain-user-profile-describe"></a>

 The following section describes how to view the details of a user profile from the SageMaker console or the AWS CLI\. 

### View user profile details from the console<a name="domain-user-profile-describe-console"></a>

 Complete the following procedure to view the details of a user profile from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to view a list of user profiles for\. 

1. On the **Domain details** page, choose the **User profiles** tab\. 

1.  Select the user profile that you want to view details for\. 

### View user profile details from the AWS CLI<a name="domain-user-profile-describe-cli"></a>

To describe a user profile from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws sagemaker describe-user-profile \
--region region \
--domain-id domain-id \
--user-profile-name user-name
```