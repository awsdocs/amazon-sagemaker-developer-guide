# JupyterLab Versioning<a name="studio-jl"></a>

The Amazon SageMaker Studio interface is based on JupyterLab, which is a web\-based interactive development environment for notebooks, code, and data\. Studio now supports using both JupyterLab 1 and JupyterLab 3\. The default version of JupyterLab in Studio is JupyterLab 3\. If you created your domain and user profile before 08/31/2022, then your Studio instance defaults to JupyterLab 1\. After 08/31/2022, JupyterLab version 1 on SageMaker Studio only receives security fixes\. You can choose the version that you want to run\. However, you can run only a single instance of JupyterLab at one time\. You can’t run multiple versions of JupyterLab simultaneously\.

**Topics**
+ [JupyterLab 3](#jl3)
+ [Restricting default JupyterLab version using an IAM policy condition key](#iam-policy)
+ [Setting a default JupyterLab version](#studio-jl-set)
+ [View and update the JupyterLab version of an app from the console](#studio-jl-view)
+ [Installing JupyterLab and Jupyter Server extensions](#studio-jl-install)

## JupyterLab 3<a name="jl3"></a>

JupyterLab 3 includes the following features that are not available in previous versions\. For more information about these features, see [JupyterLab 3\.0 is released\!](https://blog.jupyter.org/jupyterlab-3-0-is-out-4f58385e25bb)\. 
+ Visual debugger when using the Base Python 2\.0 and Data Science 2\.0 kernels\.
+ File browser filter 
+ Table of Contents \(TOC\) 
+ Multi\-language support 
+ Simple mode 
+ Single interface mode 

### Important changes to JupyterLab 3<a name="jl3-changes"></a>

 Consider the following when using JupyterLab 3: 
+ When setting the JupyterLab version using the AWS CLI, select the corresponding image for your Region and JupyterLab version from the image list in [From the AWS CLI](#studio-jl-set-cli)\.
+ In JupyterLab 3, you must activate the `studio` conda environment before installing extensions\. For more information, see [Installing JupyterLab and Jupyter Server extensions](#studio-jl-install)\.
+ Debugger is only supported when using the following images: 
  + Base Python 2\.0
  + Data Science 2\.0

## Restricting default JupyterLab version using an IAM policy condition key<a name="iam-policy"></a>

You can use IAM policy condition keys to restrict the version of JupyterLab that your users can launch\.

The following policy shows how to limit the JupyterLab version at the domain level\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Block users from creating JupyterLab 3 apps at the domain level",
            "Effect": "Deny",
            "Action": [
                "sagemaker:CreateDomain",
                "sagemaker:UpdateDomain"
            ],
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringLike": {
                    "sagemaker:ImageArns": "*image/jupyter-server-3"
                }
            }
        }
    ]
}
```

The following policy shows how to limit the JupyterLab version at the user profile level\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Block users from creating JupyterLab 3 apps at the user profile level",
            "Effect": "Deny",
            "Action": [
                "sagemaker:CreateUserProfile",
                "sagemaker:UpdateUserProfile"
            ],
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringLike": {
                    "sagemaker:ImageArns": "*image/jupyter-server-3"
                }
            }
        }
    ]
}
```

The following policy shows how to limit the JupyterLab version at the application level\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Block users from creating JupyterLab 3 apps at the application level",
            "Effect": "Deny",
            "Action": "sagemaker:CreateApp",
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringLike": {
                    "sagemaker:ImageArns": "*image/jupyter-server-3"
                }
            }
        }
    ]
}
```

## Setting a default JupyterLab version<a name="studio-jl-set"></a>

The following sections show how to set a default JupyterLab version for Studio using either the console or the AWS CLI\.  

### From the console<a name="studio-jl-set-console"></a>

 You can select the default JupyterLab version to use on either the domain or user profile level during resource creation\. To set the default JupyterLab version using the console, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.  

### From the AWS CLI<a name="studio-jl-set-cli"></a>

 You can select the default JupyterLab version to use on either the domain or user profile level using the AWS CLI\.  

 To set the default JupyterLab version using the AWS CLI, you must include the ARN of the desired default JupyterLab version as part of an AWS CLI command\. This ARN differs based on the version and the Region of the SageMaker domain\.  

The following table lists the ARNs of the available JupyterLab versions for each Region:


|  |  |  | 
| --- |--- |--- |
|  Region  |  JL1  |  JL3  | 
|  us\-east\-1  |  arn:aws:sagemaker:us\-east\-1:081325390199:image/jupyter\-server  |  arn:aws:sagemaker:us\-east\-1:081325390199:image/jupyter\-server\-3  | 
|  us\-east\-2  |  arn:aws:sagemaker:us\-east\-2:429704687514:image/jupyter\-server  |  arn:aws:sagemaker:us\-east\-2:429704687514:image/jupyter\-server\-3  | 
|  us\-west\-1  |  arn:aws:sagemaker:us\-west\-1:742091327244:image/jupyter\-server  |  arn:aws:sagemaker:us\-west\-1:742091327244:image/jupyter\-server\-3  | 
|  us\-west\-2  |  arn:aws:sagemaker:us\-west\-2:236514542706:image/jupyter\-server  |  arn:aws:sagemaker:us\-west\-2:236514542706:image/jupyter\-server\-3  | 
|  af\-south\-1  |  arn:aws:sagemaker:af\-south\-1:559312083959:image/jupyter\-server  |  arn:aws:sagemaker:af\-south\-1:559312083959:image/jupyter\-server\-3  | 
|  ap\-east\-1  |  arn:aws:sagemaker:ap\-east\-1:493642496378:image/jupyter\-server  |  arn:aws:sagemaker:ap\-east\-1:493642496378:image/jupyter\-server\-3  | 
|  ap\-south\-1  |  arn:aws:sagemaker:ap\-south\-1:394103062818:image/jupyter\-server  |  arn:aws:sagemaker:ap\-south\-1:394103062818:image/jupyter\-server\-3  | 
|  ap\-northeast\-2  |  arn:aws:sagemaker:ap\-northeast\-2:806072073708:image/jupyter\-server  |  arn:aws:sagemaker:ap\-northeast\-2:806072073708:image/jupyter\-server\-3  | 
|  ap\-southeast\-1  |  arn:aws:sagemaker:ap\-southeast\-1:492261229750:image/jupyter\-server  |  arn:aws:sagemaker:ap\-southeast\-1:492261229750:image/jupyter\-server\-3  | 
|  ap\-southeast\-2  |  arn:aws:sagemaker:ap\-southeast\-2:452832661640:image/jupyter\-server  |  arn:aws:sagemaker:ap\-southeast\-2:452832661640:image/jupyter\-server\-3  | 
|  ap\-northeast\-1  |  arn:aws:sagemaker:ap\-northeast\-1:102112518831:image/jupyter\-server  |  arn:aws:sagemaker:ap\-northeast\-1:102112518831:image/jupyter\-server\-3  | 
|  ca\-central\-1  |  arn:aws:sagemaker:ca\-central\-1:310906938811:image/jupyter\-server  |  arn:aws:sagemaker:ca\-central\-1:310906938811:image/jupyter\-server\-3  | 
|  eu\-central\-1  |  arn:aws:sagemaker:eu\-central\-1:936697816551:image/jupyter\-server  |  arn:aws:sagemaker:eu\-central\-1:936697816551:image/jupyter\-server\-3  | 
|  eu\-west\-1  |  arn:aws:sagemaker:eu\-west\-1:470317259841:image/jupyter\-server  |  arn:aws:sagemaker:eu\-west\-1:470317259841:image/jupyter\-server\-3  | 
|  eu\-west\-2  |  arn:aws:sagemaker:eu\-west\-2:712779665605:image/jupyter\-server  |  arn:aws:sagemaker:eu\-west\-2:712779665605:image/jupyter\-server\-3  | 
|  eu\-west\-3  |  arn:aws:sagemaker:eu\-west\-3:615547856133:image/jupyter\-server  |  arn:aws:sagemaker:eu\-west\-3:615547856133:image/jupyter\-server\-3  | 
|  eu\-north\-1  |  arn:aws:sagemaker:eu\-north\-1:243637512696:image/jupyter\-server  |  arn:aws:sagemaker:eu\-north\-1:243637512696:image/jupyter\-server\-3  | 
|  eu\-south\-1  |  arn:aws:sagemaker:eu\-south\-1:592751261982:image/jupyter\-server  |  arn:aws:sagemaker:eu\-south\-1:592751261982:image/jupyter\-server\-3  | 
|  sa\-east\-1  |  arn:aws:sagemaker:sa\-east\-1:782484402741:image/jupyter\-server  |  arn:aws:sagemaker:sa\-east\-1:782484402741:image/jupyter\-server\-3  | 

#### Create or update domain<a name="studio-jl-set-cli-domain"></a>

 You can set a default JupyterServer version at the domain level by invoking [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) or [UpdateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateDomain.html) and passing the `UserSettings.JupyterServerAppSettings.DefaultResourceSpec.SageMakerImageArn` field\. 

 The following shows how to create a domain with JupyterLab 3 as the default, using the AWS CLI: 

```
aws --region <REGION> \
sagemaker create-domain \
--domain-name <NEW_DOMAIN_NAME> \
--auth-mode <AUTHENTICATION_MODE> \
--subnet-ids <SUBNET-IDS> \
--vpc-id <VPC-ID> \
--default-user-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "arn:aws:sagemaker:<REGION>:<ACCOUNT_ID>:image/jupyter-server-3",
      "InstanceType": "system"
    }
  }
}'
```

 The following shows how to update a domain to use JupyterLab 3 as the default, using the AWS CLI: 

```
aws --region <REGION> \
sagemaker update-domain \
--domain-id <YOUR_DOMAIN_ID> \
--default-user-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "arn:aws:sagemaker:<REGION>:<ACCOUNT_ID>:image/jupyter-server-3",
      "InstanceType": "system"
    }
  }
}'
```

#### Create or update user profile<a name="studio-jl-set-cli-user"></a>

 You can set a default JupyterServer version at the user profile level by invoking [CreateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html) or [UpdateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateUserProfile.html) and passing the `UserSettings.JupyterServerAppSettings.DefaultResourceSpec.SageMakerImageArn` field\. 

 The following shows how to create a user profile with JupyterLab 3 as the default on an existing domain, using the AWS CLI: 

```
aws --region <REGION> \
sagemaker create-user-profile \
--domain-id <YOUR_DOMAIN_ID> \
--user-profile-name <NEW_USERPROFILE_NAME> \
--query UserProfileArn --output text \
--user-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "arn:aws:sagemaker:<REGION>:<ACCOUNT_ID>:image/jupyter-server-3",
      "InstanceType": "system"
    }
  }
}'
```

 The following shows how to update a user profile to use JupyterLab 3 as the default, using the AWS CLI: 

```
aws --region <REGION> \
sagemaker update-user-profile \
 --domain-id <YOUR_DOMAIN_ID> \
 --user-profile-name <EXISTING_USERPROFILE_NAME> \
--user-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "arn:aws:sagemaker:<REGION>:<ACCOUNT_ID>:image/jupyter-server-3",
      "InstanceType": "system"
    }
  }
}'
```

## View and update the JupyterLab version of an app from the console<a name="studio-jl-view"></a>

 The following shows how to view and update the JupyterLab version of an app\. 

1.  Navigate to the SageMaker control panel\. 

1.  Select a user to view their apps\. 

1.  To view the JupyterLab version of an app, select the app’s name\. 

1.  To update the JupyterLab version, select **Action**\. 

1.  From the dropdown menu, select **Change JupyterLab version**\. 

1.  From the **Studio settings** page, select the JupyterLab version from the dropdown menu\. 

1.  After the JupyterLab version for the user profile has been successfully updated, restart the JupyterServer app to make the version changes effective\. 

## Installing JupyterLab and Jupyter Server extensions<a name="studio-jl-install"></a>

The process for installing JupyterLab and Jupyter Server extensions differs depending on the JupyterLab version of your Studio instance\. In JupyterLab 1, you can open the terminal and install extensions without activating any conda environment\. In JupyterLab 3, you must activate the `studio` conda environment before installing extensions\. The method for this differs if you're installing the extensions from within Studio or using a lifecycle configuration script\.

### Installing Extension from within Studio<a name="studio-jl-install-studio"></a>

To install extensions from within Studio, you must activate the `studio` environment before you install extensions\. 

```
# Before installing extensions
conda activate studio

# Install your extensions
pip install <JUPYTER_EXTENSION>

# After installing extensions
conda deactivate
```

### Installing Extensions using a lifecycle configuration script<a name="studio-jl-install-lcc"></a>

If you're installing JupyterLab and Jupyter Server extensions in your lifecycle configuration script, you must modify your script so that it works with JupyterLab 3\. The following sections show the code needed for existing and new lifecycle configuration scripts\.

#### Existing lifecycle configuration script<a name="studio-jl-install-lcc-existing"></a>

If you're reusing an existing lifecycle configuration script that must work with both versions of JupyterLab, use the following code in your script:

```
# Before installing extension
export AWS_SAGEMAKER_JUPYTERSERVER_IMAGE="${AWS_SAGEMAKER_JUPYTERSERVER_IMAGE:-'jupyter-server'}"
if [ "$AWS_SAGEMAKER_JUPYTERSERVER_IMAGE" = "jupyter-server-3" ] ; then
   eval "$(conda shell.bash hook)"
   conda activate studio
fi;

# Install your extensions
pip install <JUPYTER_EXTENSION>


# After installing extension
if [ "$AWS_SAGEMAKER_JUPYTERSERVER_IMAGE" = "jupyter-server-3" ]; then
   conda deactivate
fi;
```

#### New lifecycle configuration script<a name="studio-jl-install-lcc-new"></a>

If you're writing a new lifecycle configuration script that only uses JupyterLab 3, you can use the following code in your script:

```
# Before installing extension
eval "$(conda shell.bash hook)"
conda activate studio


# Install your extensions
pip install <JUPYTER_EXTENSION>


conda deactivate
```