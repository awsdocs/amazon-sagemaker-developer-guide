# IAM Identity Center Groups in a Domain<a name="domain-groups"></a>

 If you use AWS IAM Identity Center \(successor to AWS Single Sign\-On\) authentication for your Amazon SageMaker Domain, you can add and edit group and user access to a Domain\. For more information about IAM Identity Center authentication, see [What is IAM Identity Center?](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)\. The following topics show how to manage IAM Identity Center users and groups that have access to a Domain\.

**Topics**
+ [View groups and users](#domain-groups-view)
+ [Add groups and users](#domain-groups-add)
+ [Remove groups](#domain-groups-add)

## View groups and users<a name="domain-groups-view"></a>

 Complete the following procedure to view a list of IAM Identity Center groups and users from the Amazon SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to open the **Domain settings** page for\. 

1.  On the **Domain details** page, choose the **Groups** tab\. 

## Add groups and users<a name="domain-groups-add"></a>

 Complete the following procedure to add groups and users to your Domain from the SageMaker console\. 

1.  On the **Groups** tab, choose **Assign users and groups**\. 

1.  On the **Assign users and groups** page, select the users and groups that you want to add\. 

1.  Choose **Assign users and groups**\. 

## Remove groups<a name="domain-groups-add"></a>

 Complete the following procedure to remove groups from your Domain from the SageMaker console\. For information about deleting a user, see [Remove user profiles](domain-user-profile-add-remove.md#domain-user-profile-remove)\. 

1.  On the **Groups** tab, choose the group that you want to remove\. 

1.  Choose **Unassign groups**\. 

1.  On the pop\-up window, choose **Yes, unassign groups**\. 

1. Enter *unassign* in the field\. 

1.  Choose **Unassign groups**\. 