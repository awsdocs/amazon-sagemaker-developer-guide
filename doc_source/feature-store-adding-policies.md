# Adding required policies to your IAM role<a name="feature-store-adding-policies"></a>

To get started with Amazon SageMaker Feature Store you must add the required policy to your role, `AmazonSageMakerFeatureStoreAccess`\. Below is a walkthrough on how to add it to your role through the console\. 

## Step 1: Access AWS Management Console<a name="feature-store-console"></a>

Sign in to the AWS Management Console and open the [IAM console](https://console.aws.amazon.com/iam/)\.

## Step 2: Choose Roles<a name="feature-store-console-role"></a>

In the navigation pane on the left, choose **Roles**\. The navigation pane is illustrated below with **Roles** outlined in red\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-role.png)

## Step 3: Find your role<a name="feature-store-console-role-search"></a>

In the search bar, enter the role you are using for Amazon SageMaker Feature Store\. Below illustrates with a red arrow where you enter your role\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-role-search.png)

## Step 4: Attach policy<a name="feature-store-console-role-attach"></a>

After you find your role, choose **Attach policies**\. Below illustrates this with a red arrow\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-attach-policy.png)

Next you will enter in the search bar the required policy\. Below illustrates the search bar you will use to enter the policy\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-attach-policy-search.png)

The policy you will need to add is `AmazonSageMakerFeatureStoreAccess`\. After you enter the policy, select the **check box** and then choose **Attach policy**\. Below illustrates the this step\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-policy-feature-store.png)

After you have attached both policies to your role, the policy should appear under your IAM role\. The following illustrates how it appears under your IAM role\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-policy.png)