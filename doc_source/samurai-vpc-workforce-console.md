# Using the SageMaker console to manage a VPC config<a name="samurai-vpc-workforce-console"></a>

You can use the [SageMaker console](https://console.aws.amazon.com/sagemaker) to add or remove a VPC configuration\. You can also delete an existing workforce\.

## Adding a VPC configuration to your workforce<a name="samurai-add-vpc-workforce"></a>

### Create a private workforce<a name="samurai-vpc-create-workforce"></a>
+ [Create a private workforce using Amazon Cognito](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-private-use-cognito.html)
+ [Create a private workforce using OpenID Connect \(OIDC\) Identity Provider\(IdP\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-private-use-oidc.html)\.

After you have created your private workforce you will now need to add a VPC configuration to it\.

1. Navigate to the [Amazon SageMaker](https://console.aws.amazon.com/sagemaker) in your console\.

1. Select **Labeling workforces** in the left panel\.

1. Select **Private** to access your private workforce\. After your **Workforce status** is **Active**, select **Add** next to **VPC**\.

1. You will be prompted to configure your VPC\. Provide the following:

   1. Your **VPC**\.

   1. **Subnets**

      1. Ensure that there exist a subnet for your VPC\.

   1. **Security groups**

      1. 
**Note**  
You cannot select more than 5 security groups\.

   1. After filling in this information choose **Confirm**\.

1. After you choose **Confirm**, you are redirected back to the **Private** page under **Labeling workforces** and see a green banner at the top that reads **Your private workforce update with VPC configuration was successfully initialized\.** The workforce status will be **Updating**\. The button next to the **Delete workforce** button is the refresh button which can be used to retrieve the latest **Workforce status**\. After the workforce status is changed to **Active**, the VPC endpoint ID is updated as well\.

## Removing a VPC configuration from your workforce<a name="samurai-remove-vpc-workforce"></a>

Use the following information to remove a VPC configuration from your workforce using the console\.

1. Navigate to the [Amazon SageMaker](https://console.aws.amazon.com/sagemaker) in your console\.

1. Select **Labeling workforces** in the left panel\.

1. Find and select your workforce\.

1. Under **Private workforce summary**, find **VPC** and select **Remove** next to it\.

1. Select **Remove**\.

## Deleting a workforce through the console<a name="samurai-delete-vpc-workforce"></a>

You should not have any teams associated with your workforce to delete it\. You can delete a workforce only if the workforce status is **Active** or **Failed**\.

Use the following information to delete a workforce using the console\.

1. Navigate to the [Amazon SageMaker](https://console.aws.amazon.com/sagemaker) in your console\.

1. Select **Labeling workforces** in the left panel\.

1. Find and select your workforce\.

1. Choose **Delete workforce**\.

1. Choose **Delete**\.