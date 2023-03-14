# Setting Defaults for a Domain<a name="domain-set-defaults"></a>

 With SageMaker, you can set default settings for your resources at the Amazon SageMaker Domain level\. These default settings are used in the creation of resources within the Domain\. The following sections list default settings for Domain and give information on using context keys when setting defaults\.

**Topics**
+ [Domain default settings](#domain-set-defaults-domains)
+ [Context keys](#domain-set-defaults-context)

## Domain default settings<a name="domain-set-defaults-domains"></a>

You can set the following defaults when creating or updating a Domain\. Values passed at the user profile and shared space level override defaults set at the Domain level\.
+ [ DefaultUserSettings ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UserSettings.html)
+ DefaultSpaceSettings
**Note**  
`DefaultSpaceSettings` only supports the use of JupyterLab 3 image ARNs for `SageMakerImageArn`\. For more information, see [JupyterLab Versioning](studio-jl.md)\.

  ```
  "DefaultSpaceSettings": { 
        "ExecutionRole": "string",
        "JupyterServerAppSettings": { 
           "DefaultResourceSpec": { 
              "InstanceType": "string",
              "LifecycleConfigArn": "string",
              "SageMakerImageArn": "string",
              "SageMakerImageVersionArn": "string"
           },
           "LifecycleConfigArns": [ "string" ]
        },
        "KernelGatewayAppSettings": { 
           "CustomImages": [ 
              { 
                 "AppImageConfigName": "string",
                 "ImageName": "string",
                 "ImageVersionNumber": number
              }
           ],
           "DefaultResourceSpec": { 
              "InstanceType": "string",
              "LifecycleConfigArn": "string",
              "SageMakerImageArn": "string",
              "SageMakerImageVersionArn": "string"
           },
           "LifecycleConfigArns": [ "string" ]
        },
        "SecurityGroups": [ "string" ]
     }
  ```

## Context keys<a name="domain-set-defaults-context"></a>

You can add context keys to the IAM policy that creates a Domain\. This restricts the values that users can pass for those fields\. The following list shows the context keys that Domain supports and where they're implemented\.
+ `sagemaker:ImageArns`
  + **Implemented as part of `DefaultUserSettings`:**`SagemakerImageArn` in `DefaultUserSettings.JupyterServerAppSettings` and `DefaultUserSettings.KernelGatewayAppSettings`\. `CustomImages` in `DefaultUserSettings.KernelGatewayAppSettings`\.
  + **Implemented as part of `DefaultSpaceSettings`:**`SagemakerImageArn` in `DefaultSpaceSettings.JupyterServerAppSettings` and `DefaultSpaceSettings.KernelGatewayAppSettings`\. `CustomImages` in `DefaultSpaceSettings.KernelGatewayAppSettings`\.
+ `sagemaker:VpcSecurityGroupIds`
  + **Implemented as part of `DefaultUserSettings`:**`SecurityGroups` in `DefaultUserSettings`\.
  + **Implemented as part of `DefaultSpaceSettings`:**`SecurityGroups` in `DefaultSpaceSettings`\.
+ `sagemaker:DomainSharingOutputKmsKey`

  **Implemented as part of `DefaultUserSettings`:**`S3KmsKeyId` in `DefaultSpaceSettings.SharingSettings`\.

 You cannot restrict users to passing incompatible values when using context keys for the defaults\. For example, the values for `SageMakerImageArn` set as part of `DefaultUserSettings` and `DefaultSpaceSettings` must be compatible\. You cannot set the following incompatible default values\. For more information about the available JupyterLab version ARNs, see [Setting a default JupyterLab version](studio-jl.md#studio-jl-set)\.
+ Only a JupyterLab version 1 ARN can be used for the `SageMakerImageArn` value in `DefaultUserSettings`
+ Only a JupyterLab version 3 ARN can be used for the `SageMakerImageArn` value in `DefaultSpaceSettings`