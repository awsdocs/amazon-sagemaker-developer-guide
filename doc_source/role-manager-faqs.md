# Role Manager FAQs<a name="role-manager-faqs"></a>

Refer to the following FAQ items for answers to commonly asked questions about Amazon SageMaker Role Manager\.

## Q\. How can I access Amazon SageMaker Role Manager?<a name="role-manager-faqs-access"></a>

A: You can access Amazon SageMaker Role Manager through multiple location in the Amazon SageMaker console\. For information about accessing role manager and using it to create a role, see [Using the role manager](role-manager-tutorial.md)\.

## Q\. What are personas?<a name="role-manager-faqs-personas"></a>

A: Personas are preconfigured groups of permissions based on common machine learning \(ML\) responsibitilies\. For example, the data science persona suggests permissions for general machine learning development and experimentation in a SageMaker environment, while the MLOps persona suggests permissions for ML activities related to operations\.

## Q\. What are ML activities?<a name="role-manager-faqs-ml-activities"></a>

A: ML activities are common AWS tasks related to machine learning with SageMaker that require specific IAM permissions\. Each persona suggests related ML activities when creating a role with Amazon SageMaker Role Manager\. ML activities include tasks such as Amazon S3 full access or searching and visualizing experiments\. For more information, see [ML activity reference](role-manager-ml-activities.md)\.

## Q\. Are the roles that I create with the role manager AWS Identity and Access Management \(IAM\) roles?<a name="role-manager-faqs-iam"></a>

A: Yes\. Roles created using the Amazon SageMaker Role Manager are IAM roles with customized access policies\. You can view created roles in the **Roles** section of the [IAM console](https://console.aws.amazon.com/iamv2/)\.

## Q\. How can I view the roles that I created using Amazon SageMaker Role Manager?<a name="role-manager-faqs-view-roles"></a>

A: You can view created roles in the **Roles** section of the [IAM console](https://console.aws.amazon.com/iamv2/)\. By default, the prefix `"sagemaker-"` is added to every role name for easier search in the IAM console\. For example, if you named your role `test-123` during role creation, your role shows up as `sagemaker-test-123` in the IAM console\.

## Q\. Can I modify a role made with Amazon SageMaker Role Manager once it is created?<a name="role-manager-faqs-modify-roles"></a>

A: Yes\. You can modify the roles and policies created by Amazon SageMaker Role Manager through the [IAM console](https://console.aws.amazon.com/iamv2/)\. For more information, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *AWS Identity and Access Management User Guide*\.

## Q\. Can I attach my own policies to roles created using Amazon SageMaker Role Manager?<a name="role-manager-faqs-attach-policies"></a>

A: Yes\. You can attach any AWS or customer\-managed IAM policies from your account to the role that you create using Amazon SageMaker Role Manager\.

## Q\. How many policies can I add to a role that I create with Amazon SageMaker Role Manager?<a name="role-manager-faqs-policy-limit"></a>

A: The maximum limit for attaching managed policies to an IAM role or user is 20\. The maximum character size limit for managed policies is 6,144\. For more information, see [IAM object quotas](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-quotas.html#reference_iam-quotas-entities) and [IAM and AWS Security Token Service quotas name requirements, and character limits](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-quotas.html)\.

## Q\. Can I add conditions to ML activities?<a name="role-manager-faqs-conditions"></a>

A: Any conditions that you provide in [Step 1\. Enter role information](role-manager-tutorial.md#role-manager-tutorial-enter-role-information) of the Amazon SageMaker Role Manager, such as subnets, security groups, or KMS keys, are automatically passed to any ML activities selected in [Step 2\. Configure ML activities](role-manager-tutorial.md#role-manager-tutorial-configure-ml-activities)\. You can also add additional conditions to ML activities if necessary\. For example, you might also add `InstanceTypes` or `IntercontainerTrafficEncryption` conditions to the Manage Training Jobs activity\. 

## Q\. Can I use tagging to manage access to any AWS resource?<a name="role-manager-faqs-tagging"></a>

A:****You can add tags to your role in [Step 3: Add additional policies and tags](role-manager-tutorial.md#role-manager-tutorial-add-policies-and-tags) of the Amazon SageMaker Role Manager\. To successfully manage AWS resources using tags, you must add the same tag to both the role and any associated policies\. For example, you can add a tag to a role and to an Amazon S3 bucket\. Then, because the role passes the tag to the SageMaker session, only a user with that role can access that S3 bucket\. You can add tags to a policy through the [IAM console](https://console.aws.amazon.com/iamv2/)\. For more information, see [Tagging IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags_roles.html) in the *AWS Identity and Access Management User Guide*\. 

## Q\. Can I use Amazon SageMaker Role Manager to create a role to access the AWS Management Console?<a name="role-manager-faqs-console-access"></a>

A: No\. However, after creating a service role in the role manager, you can go to the IAM console to edit the role and add a human access role in IAM console\.

## Q\. What is difference between a user federation role and a SageMaker execution role?<a name="role-manager-faqs-role-types"></a>

A: A user federation role is directly assumed by a user to access AWS resources such as access to the AWS Management Console\. A SageMaker execution role is assumed by the SageMaker service to perform a function on behalf of a user or an automation tool\. For example, when a user opens a Studio instance, Studio assumes the execution role associated with the user profile in order to access AWS resources on the behalf of the user\. If the user profile does not specify an execution role, then the execution role is specified at the Amazon SageMaker Domain level\. 

## Q\. If I am using a custom web application that accesses Studio through a presigned url, what role is used?<a name="role-manager-faqs-studio-presigned-url"></a>

A: If you use a custom web application to access Studio, then you have a hybrid user federation role and SageMaker execution role\. Be sure that this role has least privilege permissions for both what the user can do and what Studio can do on the associated user’s behalf\. 

## Q: Can I use Amazon SageMaker Role Manager with AWS IAM Identity Center authentication for my Studio domain?<a name="role-manager-faqs-iam-identity-center"></a>

A: AWS IAM Identity Center \(successor to AWS Single Sign\-On\) Studio Cloud Applications use a Studio execution role to grant permissions to federated users\. This execution role can be specified at the Studio IAM Identity Center user profile level or the default domain level\. User identities and groups must be synchronized into IAM Identity Center and the Studio user profile must be created with IAM Identity Center user assignment using [CreateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html)\. For more information, see [Launch Studio with IAM Identity Center](role-manager-launch-notebook.md#role-manager-launch-notebook-iam-identity-center)\.