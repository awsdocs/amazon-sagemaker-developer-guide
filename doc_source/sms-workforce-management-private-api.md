# Manage Private Workforce Using the Amazon SageMaker API<a name="sms-workforce-management-private-api"></a>

You can use Amazon SageMaker API operations to manage, update, and delete your private workforce\. For each API operation linked on this page, you can find a list of supported language\-specific SDKs and their documentation in the **See Also** section of the API documentation\.

## Find Your Workforce Name<a name="sms-workforce-management-private-api-name"></a>

Some of the SageMaker workforce\-related API operations require your workforce name as input\. You can see your Amazon Cognito or OIDC IdP private and vendor workforce names in an AWS Region using the []() API operation in that AWS Region\. 

If you created your workforce using your own OIDC IdP, you can find your workforce name in the Ground Truth area of the SageMaker console\. 

**To find your workforce name in the SageMaker console**

1. Go to the Ground Truth area of the SageMaker console: [https://console\.aws\.amazon\.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\.

1. Select **Labeling workforces**\.

1. Select **Private**\.

1. In the **Private workforce summary** section, locate your workforce ARN\. Your workforce name is located at the end of this ARN\. For example, if the ARN is `arn:aws:sagemaker:us-east-2:111122223333:workforce/example-workforce`, the workforce name is `example-workforce`\. 

## Restrict Worker Access to Tasks to Allowable IP Addresses<a name="sms-workforce-management-private-api-cidr"></a>

By default, a workforce isn't restricted to specific IP addresses\. You can use the [ `UpdateWorkforce`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateWorkforce.html) operation to require that workers use a specific range of IP addresses \([CIDRs](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\) to access tasks\. If you specify one or more CIDRs, workers who attempt to access tasks using any IP address outside the specified ranges are denied and get a `Not Found` error message on the worker portal\. You can specify up to 10 CIDR values using `UpdateWorkforce`\. 

After you have restricted your workforce to one or more CIDRs, the output of `UpdateWorkforce` lists all allowable CIDRs\. You can also use the [ `DescribeWorkforce`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeWorkforce.html) operation to view all allowable CIDRs for a workforce\. 

## Update OIDC Identity Provider Workforce Configuration<a name="sms-workforce-management-private-api-update"></a>

You may want to update a workforce created using your own OIDC IdP to specify a different authorization endpoint, token endpoint, or issuer\. You can update any parameter found in `[OidcConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OidcConfig.html)` using the [ `UpdateWorkforce`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateWorkforce.html) operation\.

**Important**  
You can only update your OIDC IdP configuration when there are no work teams associated with your workforce\. You can delete a private work team using the `[DeleteWorkteam](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteWorkteam.html)` operation\.

## Delete a Private Workforce<a name="sms-workforce-management-private-api-delete"></a>

You can only have one private workforce in each AWS Region\. You may want to delete your private workforce in an AWS Region when:
+ You want to create a workforce using a new Amazon Cognito user pool\. 
+ You have already created a private workforce using Amazon Cognito and you want to create a workforce using your own OpenID Connect \(OIDC\) Identity Provider \(IdP\)\.

To delete a private workforce, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteWorkforce.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteWorkforce.html) API operation\. If you have any work teams associated with your workforce, you must delete those work teams before you delete your workforce\. You can delete a private work team using the `[DeleteWorkteam](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteWorkteam.html)` operation\. 