# Permissions and Security in Amazon Augmented AI<a name="a2i-permissions-security"></a>

When using Amazon Augmented AI \(Amazon A2I\) to create a human review workflow for your ML/AI application, you create and configure *resources* in Amazon SageMaker such as a human workforce and worker task templates\. To configure and start a human loop, you will either integrate Amazon A2I with other AWS services such as Amazon Textract or Amazon Rekognition or use the Amazon Augmented AI Runtime API\. To create a human review workflow and start a human loop, you will need to attach certain policies to your AWS Identity and Access Management \(IAM\) role or user\. Specifically: 
+ When you create a flow definition, you need to provide a role that grants Amazon A2I permission to access Amazon S3 both for reading objects that will be rendered in a human task UI and for writing the results of the human review\. 

  This role will also need to have a trust policy attached to give SageMaker permission to assume the role\. This allows Amazon A2I to perform actions in accordance with permissions that you attach to the role\. 

  See [Add Permissions to the IAM Role Used to Create a Flow Definition](#a2i-human-review-permissions-s3) for example policies that you can modify and attach to the role you use to create a flow definition\. These are the policies that will be attached to the IAM role that is created in the Human review workflows section of the Amazon A2I area of the SageMaker console\. 
+ To create and start human loops, you either use an API operation from a built\-in task type \(such as `DetectModerationLabel` or `AnalyzeDocument`\) or the Amazon A2I Runtime API operation `StartHumanLoop` in a custom ML application\. You need to attach the **AmazonAugmentedAIFullAccess** managed policy to the IAM user that invokes these API operations to grant permission to these services to use Amazon A2I operations\. To learn how, see [Create an IAM User That Can Invoke Amazon A2I API Operations](#create-user-grants)\.

  This policy does *not* grant permission to invoke the API operations of the AWS service associated with built\-in task types\. For example, AmazonAugmentedAIFullAccess does not grant permission to call Amazon Rekognition API operation `DetectModerationLabel` or Amazon Textract API operation `AnalyzeDocument`\. You can use the more general policy, **AmazonAugmentedAIIntegratedAPIAccess**, to grant these permissions\. For more information, see [Create an IAM User With Permissions to Invoke Amazon A2I, Amazon Textract, and Amazon Rekognition API Operations](#a2i-grant-general-permission)\. This is a good option when you want to grant an IAM user broad permissions to use Amazon A2I and integrated AWS services' API operations\. 

  If you want to configure more granular permissions, see [Amazon Rekognition Identity\-Based Policy Examples](https://docs.aws.amazon.com/rekognition/latest/dg/security_iam_id-based-policy-examples.html) and [Amazon Textract Identity\-Based Policy Examples](https://docs.aws.amazon.com/textract/latest/dg/security_iam_id-based-policy-examples.html) for identity\-based policies you can use to grant permission to use these individual services\.
+ To preview your custom worker task UI template, you need an IAM role with permissions to read Amazon S3 objects that get rendered on your user interface\. See a policy example in [Enable Worker Task Template Previews ](#permissions-for-worker-task-templates-augmented-ai)\.

**Topics**
+ [Add Permissions to the IAM Role Used to Create a Flow Definition](#a2i-human-review-permissions-s3)
+ [Create an IAM User That Can Invoke Amazon A2I API Operations](#create-user-grants)
+ [Create an IAM User With Permissions to Invoke Amazon A2I, Amazon Textract, and Amazon Rekognition API Operations](#a2i-grant-general-permission)
+ [Enable Worker Task Template Previews](#permissions-for-worker-task-templates-augmented-ai)
+ [Additional Permissions and Security Resources](#additional-security-resources-augmented-ai)

## Add Permissions to the IAM Role Used to Create a Flow Definition<a name="a2i-human-review-permissions-s3"></a>

To create a flow definition, attach the policies in this section to the role that you use when creating a human review workflow in the SageMaker console, or using the `CreateFlowDefinition` API operation\.
+ If you are using the console to create a human review workflow, enter the role Amazon Resource Name \(ARN\) in the **IAM role** field when [creating a human review workflow in the console](https://docs.aws.amazon.com/sagemaker/latest/dg/create-human-review-console.html)\. 
+ When creating a flow definition using the API, attach these policies to the role that is passed to the `RoleArn` parameter of the `CreateFlowDefinition` operation\. 

When you create a human review workflow \(flow definition\), Amazon A2I invokes Amazon S3 to complete your task\. To grant Amazon A2I permission to retrieve and store your files in your Amazon S3 bucket, create the following policy and attach it to your role\. For example, if the images, documents, and other files that you are sending for human review are stored in an S3 bucket named `my_input_bucket`, and if you want the human reviews to be stored in a bucket named `my_output_bucket`, you would create the following policy\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::my_input_bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::my_output_bucket/*"
            ]
        }
    ]
}
```

In addition, the IAM role must have the following trust policy to give SageMaker permission to assume the role\. To learn more about IAM trust policies, see [Resource\-Based Policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based) section of Policies and Permissions in the *AWS Identity and Access Management* documentation\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSageMakerToAssumeRole",
      "Effect": "Allow",
      "Principal": {
        "Service": "sagemaker.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

For more information about creating and managing IAM roles and policies, see the following topics in the *AWS Identity and Access Management User Guide*: 
+ To create IAM role, see [Creating a Role to Delegate Permissions to an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)\. 
+ To learn how to create IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\. 
+ To learn how to attach an IAM policy to a role, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.

## Create an IAM User That Can Invoke Amazon A2I API Operations<a name="create-user-grants"></a>

To use Amazon A2I to create and start human loops for Amazon Rekognition, Amazon Textract, or the Amazon A2I runtime API, you must use an IAM user that has permissions to invoke Amazon A2I operations\. To do this, use the IAM console to attach the [https://console.aws.amazon.com/iam/home?region=us-east-2#/policies/arn:aws:iam::aws:policy/AmazonAugmentedAIFullAccess$jsonEditor](https://console.aws.amazon.com/iam/home?region=us-east-2#/policies/arn:aws:iam::aws:policy/AmazonAugmentedAIFullAccess$jsonEditor) managed policy to a new or existing IAM user\. 

This policy grants permission to an IAM user to invoke API operations from the SageMaker API for flow definition creation and management and the Amazon Augmented AI Runtime API for human loop creation and management\. To learn more about these API operations, see [Use APIs in Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-api-references.html)\.

AmazonAugmentedAIFullAccess does not grant permissions to use Amazon Rekognition or Amazon Textract API operations\. 

**Note**  
You can also attach the **AmazonAugmentedAIFullAccess** to an IAM role that is used to create and start a human loop\. 

**To create the required IAM user**

1. Sign in to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users** and choose an existing user, or create a new user by choosing **Add user**\. To learn how to create a new user, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *AWS Identity and Access Management User Guide*\.
   + If you chose to attach the policy to an existing user, choose **Add permissions**\.
   + While creating a new user, follow the next step on the **Set permissions** page\.

1. Choose **Attach existing policies directly**\.

1. In the **Search** bar, enter **AmazonAugmentedAIFullAccess** and check the box next to that policy\. 

   To enable this IAM user to create a flow definition with the public work team, also attach the **AmazonSageMakerMechanicalTurkAccess** managed policy\. 

1. After attaching the policy or policies:

   1. If you are using an existing user, choose **Next: Review**, and then choose **Add permissions**\.

   1. If you are creating a new user, choose **Next: Tags** and complete the process of creating your user\. 

For more information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *AWS Identity and Access Management User Guide*\.

## Create an IAM User With Permissions to Invoke Amazon A2I, Amazon Textract, and Amazon Rekognition API Operations<a name="a2i-grant-general-permission"></a>

To create an IAM user that has permission to invoke the API operations used by the built\-in task types \(that is, `DetectModerationLables` for Amazon Rekognition and `AnalyzeDocument` for Amazon Textract\) and permission to use all Amazon A2I API operations, attach the IAM managed policy, **AmazonAugmentedAIIntegratedAPIAccess**\. You may want to use this policy when you want to grant broad permissions to an IAM user using Amazon A2I with more than one task type\. To learn more about these API operations, see [Use APIs in Amazon Augmented AI](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-api-references.html)\.

**Note**  
You can also attach the **AmazonAugmentedAIIntegratedAPIAccess** to an IAM role that is used to create and start a human loop\. 

**To create the required IAM user**

1. Sign in to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users** and choose an existing user, or create a new user by choosing **Add user**\. To learn how to create a new user, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *AWS Identity and Access Management User Guide*\.
   + If you chose to attach the policy to an existing user, choose **Add permissions**\.
   + While creating a new user, follow the next step on the **Set permissions** page\.

1. Choose **Attach existing policies directly**\.

1. In the **Search** bar, enter **AmazonAugmentedAIIntegratedAPIAccess** and select the box next to that policy\. 

   To allow this IAM user to create a flow definition using Amazon Mechanical Turk, also attach the **AmazonSageMakerMechanicalTurkAccess** managed policy\. 

1. After attaching the policy or policies:

   1. If you are using an existing user, choose **Next: Review**, and then choose **Add permissions**\.

   1. If you are creating a new user, choose **Next: Tags** and complete the process of creating your user\. 

For more information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *AWS Identity and Access Management User Guide*\.

## Enable Worker Task Template Previews<a name="permissions-for-worker-task-templates-augmented-ai"></a>

To customize the interface and instructions that your workers see when working on your tasks, you create a worker task template\. You can create the template using the [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html) operation or the SageMaker console\. 

To preview your template, you need an IAM role with the following permissions to read Amazon S3 objects that get rendered on your user interface\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::my_input_bucket/*"
            ]
        }
    ]
}
```

For Amazon Rekognition and Amazon Textract task types, you can preview your template using the Amazon Augmented AI section of the SageMaker console\. For custom task types, you preview your template by invoking the [ `RenderUiTemplate`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html) operation\. To preview your template, follow the instructions for your task type:
+  Amazon Rekognition and Amazon Textract task types – In the SageMaker console, use the role 's Amazon Resource Name \(ARN\) in the procedure documented in [Create a Worker Task Template](a2i-worker-template-console.md#a2i-create-worker-template-console)\.
+ Custom task types – In the `RenderUiTemplate` operation, use the role's ARN in the `RoleArn` parameter\.

## Additional Permissions and Security Resources<a name="additional-security-resources-augmented-ai"></a>
+ [Control Access to SageMaker Resources by Using Tags](security_iam_id-based-policy-examples.md#access-tag-policy)\.
+ [SageMaker Identity\-Based Policies](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies)
+ [Control Creation of SageMaker Resources with Condition Keys](security_iam_id-based-policy-examples.md#sagemaker-condition-examples)
+ [Amazon SageMaker API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)
+ [Security in Amazon SageMaker](security.md)