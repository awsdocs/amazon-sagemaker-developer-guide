# Domain resource isolation<a name="domain-resource-isolation"></a>

You can isolate resources between each of the Domains in your account and Region using an AWS Identity and Access Management policy\. With this resource isolation, resources in one Domain are not visible from within another Domain\. The following topic shows how to create a new IAM policy that limits access to resources in the Domain to user profiles with the Domain tag, as well as how to attach this policy to the IAM execution role of the Domain\. You must repeat this process for each Domain in your account\. 

## Console<a name="domain-resource-isolation-console"></a>

The following section shows how to create a new IAM policy that limits access to resources in the Domain to user profiles with the Domain tag, as well as how to attach this policy to the IAM execution role of the Domain, from the Amazon SageMaker console\. 

1. Create an IAM policy named `StudioDomainResourceIsolationPolicy-domain-id` with the following JSON policy document by completing the steps in [Creating IAM policies \(console\)\.](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "CreateUpdateRequireDomainTag",
               "Effect": "Allow",
               "Action": [
                   "SageMaker:Create*",
                   "SageMaker:Update*"
               ],
               "Resource": "*",
               "Condition": {
                   "ForAllValues:StringEquals": {
                       "aws:TagKeys": [
                           "sagemaker:domain-arn"
                       ]
                   }
               }
           },
           {
               "Sid": "ResourceAccessRequireDomainTag",
               "Effect": "Allow",
               "Action": [
                   "SageMaker:Update*",
                   "SageMaker:Delete*",
                   "SageMaker:Describe*"
               ],
               "Resource": "*",
               "Condition": {
                   "StringEquals": {
                       "aws:ResourceTag/sagemaker:domain-arn": "arn:aws:sagemaker:region:account-id:domain/domain-id"
                   }
               }
           }
       ]
   }
   ```

1. Attach the `StudioDomainResourceIsolationPolicy-domain-id` policy to the Domain's execution role by completing the steps in [Modifying a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy)\. 

## AWS CLI<a name="domain-resource-isolation-cli"></a>

The following section shows how to create a new IAM policy that limits access to resources in the Domain to user profiles with the Domain tag, as well as how to attach this policy to the execution role of the Domain, from the AWS CLI\.  

1. Create a file named `StudioDomainResourceIsolationPolicy-domain-id` with the following content from your local machine\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "CreateUpdateRequireDomainTag",
               "Effect": "Allow",
               "Action": [
                   "SageMaker:Create*",
                   "SageMaker:Update*"
               ],
               "Resource": "*",
               "Condition": {
                   "ForAllValues:StringEquals": {
                       "aws:TagKeys": [
                           "sagemaker:domain-arn"
                       ]
                   }
               }
           },
           {
               "Sid": "ResourceAccessRequireDomainTag",
               "Effect": "Allow",
               "Action": [
                   "SageMaker:Update*",
                   "SageMaker:Delete*",
                   "SageMaker:Describe*"
               ],
               "Resource": "*",
               "Condition": {
                   "StringEquals": {
                       "aws:ResourceTag/sagemaker:domain-arn": "arn:aws:sagemaker:region:account-id:domain/domain-id"
                   }
               }
           }
       ]
   }
   ```

1.  Create a new IAM policy using the `StudioDomainResourceIsolationPolicy-domain-id` file\. 

   ```
   aws iam create-policy --policy-name StudioDomainResourceIsolationPolicy-domain-id --policy-document file://StudioDomainResourceIsolationPolicy-domain-id
   ```

1.  Attach the newly created policy to a new or existing role that is used as the Domain's execution role\. 

   ```
   aws iam attach-role-policy --policy-arn arn:aws:iam:account-id:policy/StudioDomainResourceIsolationPolicy-domain-id --role-name domain-execution-role
   ```