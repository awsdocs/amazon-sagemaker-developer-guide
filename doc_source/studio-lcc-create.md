# Creating and Associating a Lifecycle Configuration<a name="studio-lcc-create"></a>

Amazon SageMaker Studio apps are interactive applications that enable Studio's visual interface, code authoring, and execution experience\. App types can be either JupyterServer or KernelGateway\. 
+ **JupyterServer apps:** This app type enables access to the visual interface for Studio\. Every user in Studio gets their own JupyterServer app\. 
+ **KernelGateway apps:** This app type enables access to the code execution environment and kernels for your Studio notebooks and terminals\. For more information, see [Jupyter Kernel Gateway](https://jupyter-kernel-gateway.readthedocs.io/en/latest/)\.

For more information about Studio's architecture and Studio apps, see [Use Amazon SageMaker Studio Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/notebooks.html)\.

**Topics**
+ [Prerequisites](#prerequisites)
+ [Step 1: Create a new Lifecycle Configuration](#create-step1)
+ [Step 2: Attach the Lifecycle Configuration to your Studio Domain or UserProfile](#create-step2)
+ [Step 3: Choose a Lifecycle Configuration while launching a new App](#create-kgw)
+ [Step 4: View logs for a Lifecycle Configuration](#create-logs)

## Prerequisites<a name="prerequisites"></a>
+ Ensure your AWS CLI is up to date using the steps in [Installing the current AWS CLI Version](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html#install-tool-bundled)\.
+ From your local machine, run `aws configure` and provide your AWS credentials\. For information on AWS credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 
+ Onboard to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\.

## Step 1: Create a new Lifecycle Configuration<a name="create-step1"></a>

The following procedure shows how to create a Lifecycle Configuration script that prints `Hello World`\.

1. From your local machine, create a file named `my-script.sh` with the following content\.

   ```
   #!/bin/bash 
   set -eux 
   echo 'Hello World!'
   ```

1. Convert your `my-script.sh` file into Base64 format\. This requirement prevents errors due to the encoding of spacing and line breaks\.

   ```
   LCC_CONTENT=`openssl base64 -in my-script.sh`
   ```

1. Create a Studio Lifecycle Configuration\. The following command creates a Lifecycle Configuration that runs on launch of an associated KernelGateway App\. 

   ```
   aws sagemaker create-studio-lifecycle-config \
   --region <your-region> \
   --studio-lifecycle-config-name my-studio-lcc \
   --studio-lifecycle-config-content $LCC_CONTENT \
   --studio-lifecycle-config-app-type KernelGateway
   ```

   Note the ARN of the newly created Lifecycle Configuration that is returned\. This ARN is required to attach the Lifecycle Configuration to your App\.

## Step 2: Attach the Lifecycle Configuration to your Studio Domain or UserProfile<a name="create-step2"></a>

You need to update the UserSettings for your Studio Domain or an individual UserProfile to attach the Lifecycle Configuration\. Lifecycle Configuration scripts associated at the Domain level are inherited by all users, while those associated at the UserProfile level are scoped to a specific user\. 

The following example shows how to create a new UserProfile with the Lifecycle Configuration attached\. If you want to update an existing UserProfile, use the `update-user-profile` command instead\.

Add the Lifecycle Configuration ARN from the previous step to the settings for the appropriate AppType\. For example, place it in the `JupyterServerAppSettings` of the user\. You can add multiple Lifecycle Configurations at a time by using a list of Lifecycle Configurations\.

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

## Step 3: Choose a Lifecycle Configuration while launching a new App<a name="create-kgw"></a>

After you have attached a Lifecycle Configuration to a UserProfile, the user can select it when launching an App\. The two methods for launching an App are using the AWS CLI and via the Studio Launcher\. The following sections describe how to launch an app using these two methods\.

### Launching an App using the AWS CLI<a name="create-kgw-cli.title"></a>

Launch the app and specify the Lifecycle Configuration ARN in the `ResourceSpec` argument of the CreateApp API\. 
+ The following example shows how to create a JupyterServer App\. When creating a JupyterServer app, the app\-name must be `default`\.

  ```
  aws sagemaker create-app --domain-id <DOMAIN-ID> \
  --region <YOUR-REGION> \
  --user-profile-name <USERPROFILE-NAME> \
  --app-type JupyterServer \
  --resource-spec LifecycleConfigArn=<LIFECYCLE-CONFIGURATION-ARN> \
  --app-name default
  ```
+ The following example shows how to create a KernelGateway App\.

  ```
  aws sagemaker create-app --domain-id <DOMAIN-ID> \
  --region <YOUR-REGION> \
  --user-profile-name <USERPROFILE-NAME> \
  --resource-spec LifecycleConfigArn=<LIFECYCLE-CONFIGURATION-ARN>,SageMakerImageArn=<SAGEMAKER-IMAGE-ARN>,InstanceType=<INSTANCE-TYPE> \
  --app-type KernelGateway \
  --app-name <APP-NAME>
  ```

### Launch a KernelGateway App using the Studio Launcher<a name="create-kgw-launcher"></a>

1. Launch the Studio Domain\. For more information, see [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)\.

1. In the launcher, navigate to the `Notebooks and compute resources` section\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-lcc-launcher.png)

1. Select your SageMaker Image\.

1. Select a start\-up script\. If there is no default Lifecycle Configuration, this value defaults to `No script`\. Otherwise, this value is equal to your default Lifecycle Configuration\. Once you select a Lifecycle Configuration, you can view the entire script\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-lcc-select-lcc.png)

1. Select `Notebook` to launch a new notebook kernel with your selected image and Lifecycle Configuration\.

## Step 4: View logs for a Lifecycle Configuration<a name="create-logs"></a>

You can view the logs for your Lifecycle Configuration after it has been attached to a Studio Domain or UserProfile\. 

1. To view the CloudWatch logs for your Lifecycle Configuration, you must first provide access to CloudWatch for your IAM role\. You need read permissions for the following log group `/aws/sagemaker/studio` and the following log stream `{Domain}/{UserProfile}/{AppType}/{AppName}/LifecycleConfigOnStart`\. For information on adding permissions, see [Enabling logging from certain AWS services](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AWS-logs-and-resource-policy.html)\.

1. To monitor a Lifecycle Configuration, navigate to the `Running instances` ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid@2x.png) tab\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-lcc-running.png)

1. Select an app from the list of running apps\. Apps with attached Lifecycle Configurations have an attached indicator icon ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-lcc-indicator-icon.png)\.

1. Click the indicator icon for your app\. This opens a new panel that lists the Lifecycle Configurations\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-lcc-indicator.png)

1. From the new panel, select `View logs`\. This opens a new tab that displays the logs\.