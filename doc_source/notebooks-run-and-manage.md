# Manage and Share Amazon SageMaker Studio Notebooks<a name="notebooks-run-and-manage"></a>

These topics describe the operations you can perform from within an Amazon SageMaker Studio notebook\. The topics make references to the following screenshot of the top of a Studio notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebooks-manage-callouts.png)

**Topics**
+ [Get App Metadata](#notebooks-run-and-manage-metadata)
+ [Change an Instance Type](#notebooks-run-and-manage-switch-instance-type)
+ [Change a SageMaker Image](#notebooks-run-and-manage-change-environment)
+ [Share and Use a Studio Notebook](#notebooks-sharing)

## Get App Metadata<a name="notebooks-run-and-manage-metadata"></a>

When you create a notebook in Amazon SageMaker Studio, the App metadata is written to a file named `resource-metadata.json` in the folder `/opt/ml/metadata/`\. You can get the App metadata from within the notebook or by opening an Image terminal through the Studio Launcher\. The metadata includes:
+ **AppType** – Always "KernelGateway"
+ **DomainId** – Same as the StudioID
+ **UserProfileName** – The profile name of the current user
+ **ResourceArn** – The Amazon Resource Name \(ARN\) of the App, which includes the instance type
+ **ResourceName** – The name of the SageMaker image

**To get the App metadata**

1. In the center of the notebook menu, choose the square with the dollar sign \($\) inside\. This opens a terminal in the SageMaker image that the notebook runs in\. You can also open an Image terminal from the Studio Launcher\. In this case, make sure you choose the SageMaker image that matches the notebook\.

1. Run the following commands to display the contents of the `resource-metadata.json` file\.

   ```
   cd ../opt/ml/metadata/
   cat resource-metadata.json
   ```

The file should look similar to the following\.

```
{
    "AppType": "KernelGateway",
    "DomainId": "d-xxxxxxxxxxxx",
    "UserProfileName": "profile-name",
    "ResourceArn": "arn:aws:sagemaker:us-east-2:account-id:app/d-xxxxxxxxxxxx/profile-name/KernelGateway/datascience--1-0-ml-t3-medium",
    "ResourceName": "datascience--1-0-ml"
}
```

## Change an Instance Type<a name="notebooks-run-and-manage-switch-instance-type"></a>

 With Amazon SageMaker Studio notebooks, you can change the Amazon Elastic Compute Cloud \(Amazon EC2\) instance type that your notebook runs on from within the notebook\. 

 When you open a new notebook for the first time, you are assigned a default instance type to run the notebook\. When you open additional notebooks on the same instance type, the notebooks run on the same instance as the first notebook, even if the notebooks are using different kernels\. 

**Note**  
If you change the instance type, existing settings for the notebook are lost and installed packages must be re\-installed\.

**To change the instance type**

1. In the upper\-right corner of the notebook, choose the processor and memory of the instance type powering your notebook\. In the screenshot, this is **2 vCPU \+ 4 GiB**\.

1. In **Select instance**, choose one of the fast launch instance types that are listed\. Or to see all instance types, switch off **Fast launch only**\. The list can be sorted by any column\.

1. After choosing a type, choose **Save and continue**\.

1. Wait for the new instance to become enabled, and then the new instance type information is displayed\.

For a list of the available instance types, see [Available Instance Types](notebooks-available-instance-types.md)\. 

## Change a SageMaker Image<a name="notebooks-run-and-manage-change-environment"></a>

 With Amazon SageMaker Studio notebooks, you can change the notebook's SageMaker image and kernel from within the notebook\.

 The current SageMaker image and kernel are displayed at the upper\-right corner of a notebook\. In the screenshot, this is **Python 3 \(Data Science\)**, where `Python 3` denotes the kernel and `Data Science` denotes the SageMaker image that contains the kernel\. The color of the circle to the right of the SageMaker image indicates the idle or busy status of the kernel\.

**To change a notebook's image**

1. In the upper\-right corner of the notebook, choose the kernel name\.

1. From the drop\-down list, choose an image\.

For a list of available SageMaker images, see [Available SageMaker Images](notebooks-available-images.md)\. 

## Share and Use a Studio Notebook<a name="notebooks-sharing"></a>

 You can share your Amazon SageMaker Studio notebooks with your colleagues\. The shared notebook is a copy\. After you share your notebook, any changes you make to your original notebook aren't reflected in the shared notebook and any changes your colleague's make in their shared copies of the notebook aren't reflected in your original notebook\. If you want to share your latest version, you must create a new snapshot and then share it\. 

### Share a Notebook<a name="notebooks-sharing-share"></a>

**To share a notebook**

1. In the upper\-right corner of the notebook, choose **Share**\.

1. \(Optional\) In **Create shareable snapshot**, choose any of the following items:
   + **Include Git repo information** – Includes a link to the Git repository that contains the notebook\. This enables you and your colleague to collaborate and contribute to the same Git repository\.
   + **Include output** – Includes all notebook output that has been saved\.
**Note**  
If you're an AWS SSO user and you don't see these options, your AWS SSO administrator probably disabled the feature\. Contact your administrator\.

1. Choose **Create**\.

1. After the snapshot is created, choose **Copy link** and then choose **Close**\.

1. Share the link with your colleague\.

After selecting your sharing options, you are provided with a URL\. You can share this link with users that have access to Amazon SageMaker Studio\. When the user opens the URL, they're prompted to log in using AWS SSO or IAM authentication\. This shared notebook becomes a copy, so changes made by the recipient will not be reproduced in your original notebook\.

### Use a Shared Notebook<a name="notebooks-sharing-using"></a>

You use a shared notebook in the same way you would with a notebook that you created yourself\. When you click a link to a shared notebook for the first time, a read\-only version of the notebook opens\. To edit the shared notebook, choose **Create a Copy**\. This copies the shared notebook to your personal storage\.

The copied notebook launches on an instance of the instance type and SageMaker image that the notebook was using when the sender shared it\. If you aren't currently running an instance of the instance type, a new instance is started\. Customization to the SageMaker image isn't shared\. You can also inspect the notebook snapshot by choosing **Snapshot Details**\.

The following are some important considerations about sharing and authentication:
+ If you have an active session, you see a read\-only view of the notebook until you choose **Create a Copy**\.
+ If you don't have an active session, you need to log in\.
+ If you use IAM to login, after you login, select your user profile then choose **Open SageMaker Studio**\. Then you need to choose the link you were sent\.
+ If you use SSO to login, after you login the shared notebook is opened automatically in Studio\.