# How Amazon SageMaker Works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to SageMaker, you should understand what IAM features are available to use with SageMaker\. To get a high\-level view of how SageMaker and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [SageMaker Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies)

## SageMaker Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. SageMaker supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\. 

Policy actions in SageMaker use the following prefix before the action: `sagemaker:`\. For example, to grant someone permission to run an SageMaker training job with the SageMaker `CreateTrainingJob` API operation, you include the `sagemaker:CreateTrainingJob` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. SageMaker defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "sagemaker:action1",
      "sagemaker:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "sagemaker:Describe*"
```



To see a list of SageMaker actions, see [Actions Defined by Amazon SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awskeymanagementservice.html#awskeymanagementservice-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>



SageMaker does not support specifying resource ARNs in a policy\.

### Condition Keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM Policy Elements: Variables and Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

SageMaker defines its own set of condition keys and also supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



SageMaker supports a number of service\-specific condition keys that you can use for fine\-grained access control for the following operations:
+ [ `CreateProcessingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html)
+ [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)
+ [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)
+ [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)
+ [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html)
+ [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html)
+ [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)
+ [ `CreateNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html)
+ [ `UpdateNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateNotebookInstance.html)

To see a list of SageMaker condition keys, see [Condition Keys for Amazon SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awskeymanagementservice.html#awskeymanagementservice-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by Amazon SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awskeymanagementservice.html#awskeymanagementservice-actions-as-permissions)\.

For examples of using SageMaker condition keys, see the following: [Control Creation of SageMaker Resources with Condition Keys](security_iam_id-based-policy-examples.md#sagemaker-condition-examples)\.

#### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of SageMaker identity\-based policies, see [Amazon SageMaker Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

### SageMaker Resource\-Based Policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

SageMaker does not support resource\-based policies\.

### Authorization Based on SageMaker Tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to SageMaker resources or pass tags in a request to SageMaker\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `sagemaker:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging SageMaker resources, see [Control Access to SageMaker Resources by Using Tags](security_iam_id-based-policy-examples.md#access-tag-policy)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Control Access to SageMaker Resources by Using Tags](security_iam_id-based-policy-examples.md#access-tag-policy)\.

### SageMaker IAM Roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

#### Using Temporary Credentials with SageMaker<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

SageMaker supports using temporary credentials\.

#### Service\-Linked Roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

SageMaker doesn't support service\-linked roles\.

#### Service Roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

SageMaker supports service roles\. 

#### Choosing an IAM Role in SageMaker<a name="security_iam_service-with-iam-roles-choose"></a>

When you create a notebook instance, processing job, training job, hosted endpoint, or batch transform job resource in SageMaker, you must choose a role to allow SageMaker to access SageMaker on your behalf\. If you have previously created a service role or service\-linked role, then SageMaker provides you with a list of roles to choose from\. It's important to choose a role that allows access to the AWS operations and resources you need\. For more information, see [SageMaker Roles ](sagemaker-roles.md)\.