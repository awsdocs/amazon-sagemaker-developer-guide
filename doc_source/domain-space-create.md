# Create a shared space<a name="domain-space-create"></a>

 The following topic demonstrates how to create a shared space in an existing Amazon SageMaker Domain\. If you created your Domain without support for shared spaces, you must add support for shared spaces to your existing Domain before you can create a shared space\. 

**Topics**
+ [Add shared space support to an existing Domain](#domain-space-add)
+ [Create from the console](#domain-space-create-console)
+ [Create from AWS CLI](#domain-space-create-cli)

## Add shared space support to an existing Domain<a name="domain-space-add"></a>

 You can use the SageMaker console or the AWS CLI to add support for shared spaces to an existing Domain\. 

### Console<a name="domain-space-add-console"></a>

 Complete the following procedure to add support for shared spaces to an existing Domain from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to open the **Domain settings** page for\. 

1.  On the **Domain details** page, choose the **Domain settings** tab\. 

1.  Choose **Edit**\. 

1.  For **Space default execution role**, set an IAM role that is used by default for all shared spaces created in the Domain\. 

1.  Choose **Next**\. 

1.  Choose **Next**\. 

1.  Choose **Next**\. 

1.  Choose **Submit**\. 

### AWS CLI<a name="domain-space-add-cli"></a>

 Run the following command from the terminal of your local machine to add default shared space settings to a Domain from the AWS CLI\. If you are adding default shared space settings to a Domain within an Amazon VPC, you must also include a list of security groups\. shared spaces only support the use of JupyterLab 3 image ARNs\. For more information, see [JupyterLab Versioning](studio-jl.md)\.

```
# Public Internet domain
aws --region region \
sagemaker update-domain \
--domain-id domain-id \
--default-space-settings "ExecutionRole=execution-role-arn,JupyterServerAppSettings={DefaultResourceSpec={InstanceType=system,SageMakerImageArn=sagemaker-image-arn}}"

# VPCOnly domain
aws --region region \
sagemaker update-domain \
--domain-id domain-id \
--default-space-settings "ExecutionRole=execution-role-arn,JupyterServerAppSettings={DefaultResourceSpec={InstanceType=system,SageMakerImageArn=sagemaker-image-arn}},SecurityGroups=[security-groups]"
```

 Verify that the default shared space settings have been updated\. 

```
aws --region region \
sagemaker describe-domain \
--domain-id domain-id
```

## Create from the console<a name="domain-space-create-console"></a>

 Complete the following procedure to create a shared space in the Domain from the SageMaker console\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select the Domain that you want to create a shared space for\. 

1.  On the **Domain details** page, choose the **Space management** tab\. 

1.  Choose **Create**\. 

1.  Enter a name for your shared space\. shared space names within a Domain must be unique\. The execution role for the shared space is set to the Domain IAM execution role\. 

## Create from AWS CLI<a name="domain-space-create-cli"></a>

This section shows how to create a shared space from the AWS CLI\. 

You cannot set the execution role of a shared space when creating or updating it\. The `DefaultDomainExecRole` can only be set when creating or updating the Domain\. shared spaces only support the use of JupyterLab 3 image ARNs\. For more information, see [JupyterLab Versioning](studio-jl.md)\.

To create a shared space from the AWS CLI, run the following command from the terminal of your local machine\.

```
aws --region region \
sagemaker create-space \
--domain-id domain-id \
--space-name space-name \
--space-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "sagemaker-image-arn",
      "InstanceType": "system"
    }
  }
}'
```
