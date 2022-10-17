# Create a Lifecycle Configuration from the AWS CLI<a name="studio-lcc-create-cli"></a>

The following topic shows how to create a lifecycle configuration using the AWS CLI to automate customization for your Studio environment\.

## Prerequisites<a name="studio-lcc-create-cli-prerequisites"></a>

Before you begin, complete the following prerequisites: 
+ Update the AWS CLI by following the steps in [Installing the current AWS CLI Version](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html#install-tool-bundled)\.
+ From your local machine, run `aws configure` and provide your AWS credentials\. For information about AWS credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 
+ Onboard to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\.

## Step 1: Create a Lifecycle Configuration<a name="studio-lcc-create-cli-step1"></a>

The following procedure shows how to create a lifecycle configuration script that prints `Hello World`\.

1. From your local machine, create a file named `my-script.sh` with the following content\.

   ```
   #!/bin/bash
   set -eux
   echo 'Hello World!'
   ```

1. Convert your `my-script.sh` file into base64 format\. This requirement prevents errors that occur from spacing and line break encoding\.

   ```
   LCC_CONTENT=`openssl base64 -A -in my-script.sh`
   ```

1. Create a Studio lifecycle configuration\. The following command creates a lifecycle configuration that runs when you launch an associated `KernelGateway` application\. 

   ```
   aws sagemaker create-studio-lifecycle-config \
   --region <your-region> \
   --studio-lifecycle-config-name my-studio-lcc \
   --studio-lifecycle-config-content $LCC_CONTENT \
   --studio-lifecycle-config-app-type KernelGateway
   ```

   Note the ARN of the newly created lifecycle configuration that is returned\. This ARN is required to attach the lifecycle configuration to your application\.

## Step 2: Attach the Lifecycle Configuration to your Studio domain or user profile<a name="studio-lcc-create-cli-step2"></a>

To attach the lifecycle configuration, you must update the `UserSettings` for your Studio domain or an individual user profile\. Lifecycle configuration scripts that are associated at the domain level are inherited by all users\. However, scripts that are associated at the user profile level are scoped to a specific user\. 

The following example shows how to create a new user profile with the lifecycle configuration attached\. To update an existing user profile, use the [update\-user\-profile](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/update-user-profile.html) command\.

Add the lifecycle configuration ARN from the previous step to the settings for the appropriate `AppType`\. For example, place it in the `JupyterServerAppSettings` of the user\. You can add multiple lifecycle configuration at the same time by using a list of lifecycle configuration\.

```
# Create a new UserProfile
aws sagemaker create-user-profile --domain-id <DOMAIN-ID> \
--user-profile-name <USER-PROFILE-NAME> \
--region <REGION> \
--user-settings '{
"JupyterServerAppSettings": {
  "LifecycleConfigArns":
    ["<LIFECYCLE-CONFIGURATION-ARN-LIST>"]
  }
}'
```

## Step 3: Launch application with Lifecycle Configuration<a name="studio-lcc-create-cli-step3"></a>

After you attach a lifecycle configuration to a user profile, the user can select it when launching an application using the AWS CLI\. This section describes how to launch an application with an attached lifecycle configuration\.

Launch the application and specify the lifecycle configuration ARN in the `ResourceSpec` argument of the `CreateApp` API\. 
+ The following example shows how to create a `JupyterServer` application\. When creating the `app-type JupyterServer`, the `app-name` must be `default`\.

  ```
  aws sagemaker create-app --domain-id <DOMAIN-ID> \
  --region <YOUR-REGION> \
  --user-profile-name <USERPROFILE-NAME> \
  --app-type JupyterServer \
  --resource-spec LifecycleConfigArn=<LIFECYCLE-CONFIGURATION-ARN> \
  --app-name default
  ```
+ The following example shows how to create a `KernelGateway` application\.

  ```
  aws sagemaker create-app --domain-id <DOMAIN-ID> \
  --region <YOUR-REGION> \
  --user-profile-name <USERPROFILE-NAME> \
  --app-type KernelGateway \
  --resource-spec LifecycleConfigArn=<LIFECYCLE-CONFIGURATION-ARN>,SageMakerImageArn=<SAGEMAKER-IMAGE-ARN>,InstanceType=<INSTANCE-TYPE> \
  --app-name <APP-NAME>
  ```