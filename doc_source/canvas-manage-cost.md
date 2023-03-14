# Manage billing and cost in SageMaker Canvas<a name="canvas-manage-cost"></a>

To track the costs associated with your SageMaker Canvas application, you can use the AWS Billing and Cost Management service\. Billing and Cost Management provides tools to help you gather information related to your cost and usage, analyze your cost drivers and usage trends, and take action to budget your spending\. For more information, see [What is AWS Billing and Cost Management?](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)\.

Billing in SageMaker Canvas consists of the following components:
+ Session charges – You are charged for the number of hours that you are logged in to or using SageMaker Canvas\.
+ Training charges – You are charged for training models based on the size of the dataset you use\.

For more information, see [SageMaker Canvas pricing](http://aws.amazon.com/sagemaker/canvas/pricing/)\.

To help you track your costs in Billing and Cost Management, you can assign custom tags to your SageMaker Canvas app and users\. You can track the costs your apps incur, and by tagging individual user profiles, you can track costs based on the user profile\. For more information about tags, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)\.

You can add tags to your SageMaker Canvas app and users by doing the following:
+ If you are setting up your Amazon SageMaker Domain and SageMaker Canvas for the first time, follow the [Getting Started](https://docs.aws.amazon.com/sagemaker/latest/dg/canvas-getting-started.html) instructions and add tags when creating your Domain or users\. You can add tags either through the **General settings** in the Domain console setup, or through the APIs \([CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) or [CreateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html)\)\. SageMaker adds the tags specified in your Domain or UserProfile to any SageMaker Canvas apps or users you create after you create the Domain\.
+ If you want to add tags to apps in an existing Domain, you must add tags to either the Domain or the UserProfile\. You can adds tags through either the console or the [AddTags](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddTags.html) API\. If you add tags through the console, then you must delete and relaunch your SageMaker Canvas app in order for the tags to propagate to the app\. If you use the API, the tags are added directly to the app\. For more information about deleting and relaunching a SageMaker Canvas app, see [Manage apps](https://docs.aws.amazon.com/sagemaker/latest/dg/canvas-manage-apps.html)\.

After you add tags to your Domain, it might take up to 24 hours for the tags to appear in the AWS Billing and Cost Management console for activation\. After they appear in the console, it takes another 24 hours for the tags to activate\.

On the **Cost explorer** page, you can group and filter your costs by tags and usage types to separate your Session charges from your Training charges\. The names of the usage types are as follows:
+ Session charges: `REGION-Canvas:Session-Hrs (Hrs)`
+ Training charges:
  + `REGION-Canvas:CreateModelRequest-Tier0 (CreateModelRequest)`
  + `REGION-Canvas:MillionCells-Tier1 (MillionCells)`