# Attach the SageMaker image to a new domain<a name="studio-byoi-sdk-attach-new-domain"></a>

To use this method, you need to specify an execution role that has the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

**Note**  
You can have only one domain\. If you have onboarded to SageMaker Studio, you must delete your current domain before you can use this method\. For more information, see [Delete an Amazon SageMaker Studio Domain](gs-studio-delete-domain.md)\.

You perform the following steps to create the domain and attach the custom SageMaker image:
+ Get your default VPC ID and subnet IDs\.
+ Create the configuration file for the domain, which specifies the image\.
+ Create the domain with the configuration file\.

**To add the custom SageMaker image to your domain**

1. Get your default VPC ID\.

   ```
   aws ec2 describe-vpcs \
       --filters Name=isDefault,Values=true \
       --query "Vpcs[0].VpcId" --output text
   ```

   Response:

   ```
   vpc-xxxxxxxx
   ```

1. Get your default subnet IDs using the VPC ID from the previous step\.

   ```
   aws ec2 describe-subnets \
       --filters Name=vpc-id,Values=<vpc-id> \
       --query "Subnets[*].SubnetId" --output json
   ```

   Response:

   ```
   [
       "subnet-b55171dd",
       "subnet-8a5f99c6",
       "subnet-e88d1392"
   ]
   ```

1. Create a configuration file named `create-domain-input.json`\. Insert the VPC ID, subnet IDs, `ImageName`, and `AppImageConfigName` from the previous steps\. Because `ImageVersionNumber` isn't specified, the latest version of the image is used, which is the only version in this case\.

   ```
   {
       "DomainName": "domain-with-custom-r-image",
       "VpcId": "<vpc-id>",
       "SubnetIds": [
           "<subnet-ids>"
       ],
       "DefaultUserSettings": {
           "ExecutionRole": "<execution-role>",
           "KernelGatewayAppSettings": {
               "CustomImages": [
                   {
                       "ImageName": "r-image",
                       "AppImageConfigName": "r-image-config"
                   }
               ]
           }
       },
       "AuthMode": "IAM"
   }
   ```

1. Create the domain with the attached custom SageMaker image\.

   ```
   aws sagemaker create-domain \
       --cli-input-json file://create-domain-input.json
   ```

   Response:

   ```
   {
       "DomainArn": "arn:aws:sagemaker:us-east-2:acct-id:domain/d-xxxxxxxxxxxx",
       "Url": "https://d-xxxxxxxxxxxx.studio.us-east-2.sagemaker.aws/..."
   }
   ```