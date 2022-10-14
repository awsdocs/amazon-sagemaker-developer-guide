# Deploy a Model<a name="jumpstart-deploy"></a>

When you deploy a model from JumpStart, SageMaker hosts the model and deploys an endpoint that you can use for inference\. JumpStart also provides an example notebook that you can use to access the model after it's deployed\. 

## Model deployment configuration<a name="jumpstart-config"></a>

After you choose a model, the **Deploy Model** pane opens\. Choose **Deployment Configuration** to configure your model deployment\. 

 ![\[Deploy Model pane option to open settings for Deployment Configuration and Security Settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy.png) 

The default instance type for deploying a model depends on the model\. The instance type is the hardware that the training job runs on\. In the following example, the `ml.g4dn.xlarge` instance is the default for this particular BERT model\. 

You can also change the **Endpoint name**\. 

 ![\[JumpStart Deploy Model pane with Deployment Configuration open to select its settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-config.png) 

Choose **Security Settings** to specify the AWS Identity and Access Management \(IAM \) role, Amazon Virtual Private Cloud \(Amazon VPC\), and encryption keys for the model\.

 ![\[JumpStart Deploy Model pane with Security Settings open to select its settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security.png) 

### Model deployment security<a name="jumpstart-config-security"></a>

When you deploy a model with JumpStart, you can specify an IAM role, Amazon VPC, and encryption keys for the model\. If you don't specify any values for these entries: The default IAM role is your Studio runtime role; default encryption is used; no Amazon VPC is used\.

#### IAM role<a name="jumpstart-config-security-iam"></a>

You can select an IAM role that is passed as part of training jobs and hosting jobs\. SageMaker uses this role to access training data and model artifacts\. If you don't select an IAM role, SageMaker deploys the model using your Studio runtime role\. For more information about IAM roles, see [Identity and Access Management for Amazon SageMaker](security-iam.md)\.

The role that you pass must have access to the resources that the model needs, and must include all of the following\.
+ For training jobs: [CreateTrainingJob API: Execution Role Permissions](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createtrainingjob-perms)\.
+ For hosting jobs: [CreateModel API: Execution Role Permissions](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createmodel-perms)\.

**Note**  
You can scope down the Amazon S3 permissions granted in each of the following roles\. Do this by using the ARN of your Amazon Simple Storage Service \(Amazon S3\) bucket and the JumpStart Amazon S3 bucket\.  

```
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:ListMultipartUploadParts"
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>/*",
    "arn:aws:s3:<region>:<account>:bucket/*",
  ]
},{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>",
    "arn:aws:s3:<region>:<account>:bucket",
  ]
```

**Find IAM role**

If you select this option, you must select an existing IAM role from the dropdown list\.

 ![\[JumpStart Security Settings IAM section with Find IAM role selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findiam.png) 

**Input IAM role**

If you select this option, you must manually enter the ARN for an existing IAM role\. If your Studio runtime role or Amazon VPC block the `iam:list* `call, you must use this option to use an existing IAM role\.

 ![\[JumpStart Security Settings IAM section with Input IAM role selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputiam.png) 

#### Amazon VPC<a name="jumpstart-config-security-vpc"></a>

All JumpStart models run in network isolation mode\. After the model container is created, no more calls can be made\. You can select an Amazon VPC that is passed as part of training jobs and hosting jobs\. SageMaker uses this Amazon VPC to push and pull resources from your Amazon S3 bucket\. This Amazon VPC is different from the Amazon VPC that limits access to the public internet from your Studio instance\. For more information about the Studio Amazon VPC, see [Connect SageMaker Studio Notebooks in a VPC to External Resources](studio-notebooks-and-internet-access.md)\.

The Amazon VPC that you pass does not need access to the public internet, but it does need access to Amazon S3\. The Amazon VPC endpoint for Amazon S3 must allow access to at least the following resources that the model needs\.

```
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:ListMultipartUploadParts"
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>/*",
    "arn:aws:s3:<region>:<account>:bucket/*",
  ]
},{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>",
    "arn:aws:s3:<region>:<account>:bucket",
  ]
```

If you do not select an Amazon VPC, no Amazon VPC is used\.

**Find VPC**

If you select this option, you must select an existing Amazon VPC from the dropdown list\. After you select an Amazon VPC, you must select a subnet and security group for your Amazon VPC\. For more information about subnets and security groups, see [Overview of VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.

 ![\[JumpStart Security Settings VPC section with Find VPC selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findvpc.png) 

**Input VPC**

If you select this option, you must manually select the subnet and security group that compose your Amazon VPC\. If your Studio runtime role or Amazon VPC blocks the `ec2:list*` call, you must use this option to select the subnet and security group\.

 ![\[JumpStart Security Settings VPC section with Input VPC selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputvpc.png) 

#### Encryption keys<a name="jumpstart-config-security-encryption"></a>

You can select an AWS KMS key that is passed as part of training jobs and hosting jobs\. SageMaker uses this key to encrypt the Amazon EBS volume for the container, and the repackaged model in Amazon S3 for hosting jobs and the output for training jobs\. For more information about AWS KMS keys, see [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#kms_keys)\.

The key that you pass must trust the IAM role that you pass\. If you do not specify an IAM role, the AWS KMS key must trust your Studio runtime role\.

If you do not select an AWS KMS key, SageMaker provides default encryption for the data in the Amazon EBS volume and the Amazon S3 artifacts\.

**Find encryption keys**

If you select this option, you must select existing AWS KMS keys from the dropdown list\.

 ![\[JumpStart Security Settings encryption section with Find encryption keys selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findencryption.png) 

**Input encryption keys**

If you select this option, you must manually enter the AWS KMS keys\. If your Studio execution role or Amazon VPC block the `kms:list* `call, you must use this option to select existing AWS KMS keys\.

 ![\[JumpStart Security Settings encryption section with Input encryption keys selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputencryption.png) 