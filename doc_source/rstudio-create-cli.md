# Create an Amazon SageMaker Domain with RStudio using the AWS CLI<a name="rstudio-create-cli"></a>

The following topic shows how to onboard to Amazon SageMaker Domain with RStudio enabled using the AWS CLI\. To onboard using the AWS Management Console, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. 

## Prerequisites<a name="rstudio-create-cli-prerequisites"></a>
+  Install and configure [AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) 
+  Configure the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config) with IAM credentials 

## Create `DomainExecution` role<a name="rstudio-create-cli-domainexecution"></a>

To launch the RStudio App, you must provide a `DomainExecution` role\. This role is used to determine whether RStudio needs to be launched as part of Amazon SageMaker Domain creation\. This role is also used by Amazon SageMaker to access the RStudio License and push RStudio logs\.  

**Note**  
The `DomainExecution` role should have at least AWS License Manager permissions to access RStudio License, and CloudWatch permissions to push logs in your account\.

The following procedure shows how to create the `DomainExecution` role with the AWS CLI\. 

1.  Create a file named `assume-role-policy.json` with the following content\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": "sts:AssumeRole",
               "Effect": "Allow",
               "Principal": {
                   "Service": [
                       "sagemaker.amazonaws.com"
                   ]
               }
           }
       ]
   }
   ```

1.  Create the `DomainExecution` role\. `<REGION>` should be the AWS Region to launch your Domain in\. 

   ```
   aws iam create-role --region <REGION> --role-name DomainExecution --assume-role-policy-document file://assume-role-policy.json
   ```

1. Create a file named `domain-setting-policy.json` with the following content\. This policy allows the RStudioServerPro app to access necessary resources and allows Amazon SageMaker to automatically launch an RStudioServerPro app when the existing RStudioServerPro app is in a `Deleted` or `Failed` status\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "license-manager:ExtendLicenseConsumption",
                   "license-manager:ListReceivedLicenses",
                   "license-manager:GetLicense",
                   "license-manager:CheckoutLicense",
                   "license-manager:CheckInLicense",
                   "logs:CreateLogDelivery",
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:DeleteLogDelivery",
                   "logs:Describe*",
                   "logs:GetLogDelivery",
                   "logs:GetLogEvents",
                   "logs:ListLogDeliveries",
                   "logs:PutLogEvents",
                   "logs:PutResourcePolicy",
                   "logs:UpdateLogDelivery",
                   "sagemaker:CreateApp"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1.  Create the Domain setting policy that is attached to the `DomainExecution` role\. Be aware of the `PolicyArn` from the response, you will need to enter that ARN in the following steps\. 

   ```
   aws iam create-policy --region <REGION> --policy-name domain-setting-policy --policy-document file://domain-setting-policy.json
   ```

1.  Attach `domain-setting-policy` to the `DomainExecution` role\. Use the `PolicyArn` returned in the previous step\.

   ```
   aws iam attach-role-policy --role-name DomainExecution --policy-arn <POLICY_ARN>
   ```

## Create Amazon SageMaker Domain with RStudio App<a name="rstudio-create-cli-domain"></a>

The RStudioServerPro app is launched automatically when you create a Amazon SageMaker Domain using the `create-domain` CLI command with the `RStudioServerProDomainSettings` parameter specified\. When launching the RStudioServerPro App, Amazon SageMaker checks for a valid RStudio license in the account and fails Domain creation if the license is not found\. 

The creation of a Amazon SageMaker Domain differs based on the authentication method and the network type\. These options must be used together, with one authentication method and one network connection type selected\. For more information about the requirements to create a new Domain, see [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html)\. 

The following authentication methods are supported\.
+  `IAM Auth` 
+  `SSO Auth` 

The following network connection types are supported:
+  `PublicInternet` 
+  `VPCOnly` 

### Authentication methods<a name="rstudio-create-cli-domain-auth"></a>

 **IAM Auth Mode** 

The following shows how to create a Amazon SageMaker Domain with RStudio enabled and an `IAM Auth` Network Type\. For more information about AWS Identity and Access Management, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.
+ `DomainExecutionRoleArn` should be the ARN for the role created in the previous step\.
+ `ExecutionRole` is the ARN of the role given to users in the Amazon SageMaker Domain\.
+ `vpc-id` should be the ID of your Amazon Virtual Private Cloud\. `subnet-ids` should be a space\-separated list of subnet IDs\. For information about `vpc-id` and `subnet-ids`, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
+ `RStudioPackageManagerUrl` and `RStudioConnectUrl` are optional and should be set to the URLs of your RStudio Package Manager and RStudio Connect server, respectively\.
+ `app-network-access-type` should be either `PublicInternetOnly` or `VPCOnly`\.

```
aws sagemaker create-domain --region <REGION> --domain-name <DOMAIN_NAME> \
    --auth-mode IAM \
    --default-user-settings ExecutionRole=<DEFAULT_USER_EXECUTIONROLE> \
    --domain-settings RStudioServerProDomainSettings={RStudioPackageManagerUrl=<<PACKAGE_MANAGER_URL>,RStudioConnectUrl=<<CONNECT_URL>,DomainExecutionRoleArn=<DOMAINEXECUTION_ROLE_ARN>} \
    --vpc-id <VPC_ID> \
    --subnet-ids <SUBNET_IDS> \
    --app-network-access-type <NETWORK_ACCESS_TYPE>
```

 **AWS SSO auth mode** 

The following shows how to create a Amazon SageMaker Domain with RStudio enabled and an `SSO Auth` Network Type\. AWS Single Sign\-On must be enabled for the region that the domain is launched on\. For more information about AWS SSO, see [What is AWS Single Sign\-On?](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)\.
+ `DomainExecutionRoleArn` should be the ARN for the role created in the previous step\.
+ `ExecutionRole` is the ARN of the role given to users in the Amazon SageMaker Domain\.
+ `vpc-id` should be the ID of your Amazon Virtual Private Cloud\. `subnet-ids` should be a space\-separated list of subnet IDs\. For information about `vpc-id` and `subnet-ids`, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
+ `RStudioPackageManagerUrl` and `RStudioConnectUrl` are optional and should be set to the URLs of your RStudio Package Manager and RStudio Connect server, respectively\.
+ `app-network-access-type` should be either `PublicInternetOnly` or `VPCOnly`\.

```
aws sagemaker create-domain --region <REGION> --domain-name <DOMAIN_NAME> \
    --auth-mode SSO \
    --default-user-settings ExecutionRole=<DEFAULT_USER_EXECUTIONROLE> \
    --domain-settings RStudioServerProDomainSettings={RStudioPackageManagerUrl=<<PACKAGE_MANAGER_URL>,RStudioConnectUrl=<<CONNECT_URL>,DomainExecutionRoleArn=<DOMAINEXECUTION_ROLE_ARN>} \
    --vpc-id <VPC_ID> \
    --subnet-ids <SUBNET_IDS> \
    --app-network-access-type <NETWORK_ACCESS_TYPE>
```

### Connection types<a name="rstudio-create-cli-domain-connection"></a>

 **PublicInternet/Direct Internet network type** 

The following shows how to create a Amazon SageMaker Domain with RStudio enabled and a `PublicInternet` Network Type\.
+ `DomainExecutionRoleArn` should be the ARN for the role created in the previous step\.
+ `ExecutionRole` is the ARN of the role given to users in the Amazon SageMaker Domain\.
+ `vpc-id` should be the ID of your Amazon Virtual Private Cloud\. `subnet-ids` should be a space\-separated list of subnet IDs\. For information about `vpc-id` and `subnet-ids`, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
+ `RStudioPackageManagerUrl` and `RStudioConnectUrl` are optional and should be set to the URLs of your RStudio Package Manager and RStudio Connect server, respectively\.
+ `auth-mode` should be either `SSO` or `IAM`\.

```
aws sagemaker create-domain --region <REGION> --domain-name <DOMAIN_NAME> \
    --auth-mode <AUTH_MODE> \
    --default-user-settings ExecutionRole=<DEFAULT_USER_EXECUTIONROLE> \
    --domain-settings RStudioServerProDomainSettings={RStudioPackageManagerUrl=<<PACKAGE_MANAGER_URL>,RStudioConnectUrl=<<CONNECT_URL>,DomainExecutionRoleArn=<DOMAINEXECUTION_ROLE_ARN>} \
    --vpc-id <VPC_ID> \
    --subnet-ids <SUBNET_IDS> \
    --app-network-access-type PublicInternetOnly
```

 **VPCOnly mode** 

The following shows how to launch a Amazon SageMaker Domain with RStudio enabled and a `VPCOnly` Network Type\. For more information about using the `VPCOnly` network access type, see [Connect SageMaker Studio Notebooks in a VPC to External Resources](studio-notebooks-and-internet-access.md)\.
+ `DomainExecutionRoleArn` should be the ARN for the role created in the previous step\.
+ `ExecutionRole` is the ARN of the role given to users in the Amazon SageMaker Domain\.
+ `vpc-id` should be the ID of your Amazon Virtual Private Cloud\. `subnet-ids` should be a space\-separated list of subnet IDs\. Your private subnet must be able to either access the internet to make a call to Amazon SageMaker, and AWS License Manager or have Amazon VPC endpoints for both Amazon SageMaker and AWS License Manager\. For information about Amazon VPC endpoints, see [Interface Amazon VPC endpoints ](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html)For information about `vpc-id` and `subnet-ids`, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\. 
+ `SecurityGroups` must allow outbound access to the Amazon SageMaker and AWS License Manager endpoints\.
+ `auth-mode` should be either `SSO` or `IAM`\.

**Note**  
When using Amazon Virtual Private Cloud endpoints, the security group attached to your Amazon Virtual Private Cloud endpoints must allow inbound traffic from the security group you pass as part of the `domain-setting` parameter of the `create-domain` CLI call\.

With RStudio, Amazon SageMaker manages security groups for you\. This means that Amazon SageMaker manages security group rules to ensure RSessions can access RStudioServerPro Apps\. Amazon SageMaker creates one security group rule per user profile\.

```
aws sagemaker create-domain --region <REGION> --domain-name <DOMAIN_NAME> \
    --auth-mode <AUTH_MODE> \
    --default-user-settings SecurityGroups=<USER_SECURITY_GROUP>,ExecutionRole=<DEFAULT_USER_EXECUTIONROLE> \
    --domain-settings SecurityGroupIds=<DOMAIN_SECURITY_GROUP>,RStudioServerProDomainSettings={DomainExecutionRoleArn=<DOMAINEXECUTION_ROLE_ARN>} \
    --vpc-id <VPC_ID> \
    --subnet-ids "<SUBNET_IDS>" \
    --app-network-access-type VPCOnly --app-security-group-management Service
```

Note: The RStudioServerPro app is launched by a special user profile named `domain-shared`\. As a result, this app is not returned as part of `list-app` API calls by any other user profiles\. 

You may have to increase the Amazon VPC quota in your account to increase the number of users\. For more information, see [Amazon VPC quotas](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html#vpc-limits-security-groups)\. 

## Verify Domain creation<a name="rstudio-create-cli-domain-verify"></a>

Use the following command to verify that your Domain has been created with a `Status` of `InService`\. Your `domain-id` is appended to the Domains ARN\. For example, `arn:aws:sagemaker:<REGION>:<ACCOUNT_ID>:domain/<DOMAIN_ID>`\.

```
aws sagemaker describe-domain --domain-id <DOMAIN_ID> --region <REGION>
```