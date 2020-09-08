# Share and Use a Amazon SageMaker Studio Notebook<a name="notebooks-sharing"></a>

You can share your Amazon SageMaker Studio notebooks with your colleagues\. The shared notebook is a copy\. After you share your notebook, any changes you make to your original notebook aren't reflected in the shared notebook and any changes your colleague's make in their shared copies of the notebook aren't reflected in your original notebook\. If you want to share your latest version, you must create a new snapshot and then share it\.

**Topics**
+ [Share a Notebook](#notebooks-sharing-share)
+ [Use a Shared Notebook](#notebooks-sharing-using)

## Share a Notebook<a name="notebooks-sharing-share"></a>

The following screenshot shows the menu from a Studio notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebooks-manage-callouts.png)

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

## Use a Shared Notebook<a name="notebooks-sharing-using"></a>

You use a shared notebook in the same way you would with a notebook that you created yourself\. When you click a link to a shared notebook for the first time, a read\-only version of the notebook opens\. To edit the shared notebook, choose **Create a Copy**\. This copies the shared notebook to your personal storage\.

The copied notebook launches on an instance of the instance type and SageMaker image that the notebook was using when the sender shared it\. If you aren't currently running an instance of the instance type, a new instance is started\. Customization to the SageMaker image isn't shared\. You can also inspect the notebook snapshot by choosing **Snapshot Details**\.

The following are some important considerations about sharing and authentication:
+ If you have an active session, you see a read\-only view of the notebook until you choose **Create a Copy**\.
+ If you don't have an active session, you need to log in\.
+ If you use IAM to login, after you login, select your user profile then choose **Open SageMaker Studio**\. Then you need to choose the link you were sent\.
+ If you use SSO to login, after you login the shared notebook is opened automatically in Studio\.