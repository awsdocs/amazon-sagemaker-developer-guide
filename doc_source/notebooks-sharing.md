# Share Amazon SageMaker Studio Notebooks<a name="notebooks-sharing"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

 You can share any of your notebooks with others in your organization\. It is a copy, so any changes you make to your notebook aren’t reflected in a previous version that you shared\. If you want to share your latest version, you must create a new snapshot and then share it\. 

 **To share a notebook** 

On the  menu directly above your notebook, choose the camera icon to create a shareable screenshot\.

Choose any of the following options: 
+ **Include github information** \- If available, the GitHub repository associated with the notebook\. This enables you and your recipient to collaborate and contribute to the same git repository\. 
+ **Include outputs** \- You can share a clean notebook, or include the output of your notebook’s last run\. Check this if you want to share the last run result, especially if the notebook takes long to run\. 
**Note**  
If you don’t see some or any of these options, your administrator probably disabled the feature\. Contact your administrator\. 

After selecting your sharing options, you are provided with a URL\. You can share this link with users that have access to Studio in your organization\. When the user opens the URL, they’re prompted to log in using AWS SSO or IAM\. This shared notebook becomes a copy, so changes made by the recipient will not occur in your original notebook\. 

## Use a Shared Notebook<a name="notebooks-sharing-using"></a>

 You use a shared notebook in the same way you would any notebook that you created yourself\. When you click a link to a shared notebook for the first time you will open a read\-only version of the notebook\. To edit, run and save the shared notebook, choose **Create a Copy** to copy the shared notebook to your personal storage\. The notebook launches with the instance and built\-in environment that the sender was using when they shared it\. Customization to the environment cannot be shared at this moment\. You can also inspect the notebook snapshot by choosing **Snapshot Details**\. 

 ​ 

 Important considerations about sharing and authentication: 

If you have an active session, you see a read\-only view of the notebook until you choose **Create a Copy**\. 

If you don’t have an active session, you need to log in\. 
+ If you use IAM to login, after you login, select your user profile then choose **Open SageMaker Studio**\. Then you need to choose the link you were sent\.  
+ If you use SSO to login, after you login the shared notebook is opened automatically in JupyterLab\. 

Notebooks are opened based on the sender’s instance type and environments \(those are passed as metadata to the notebooks\)\. A new instance of the same type is launched for you when you create a copy of the notebook\. 

## Manage Your Storage Volume<a name="notebooks-personal-storage-manage"></a>

 When you set up Amazon SageMaker Studio, an Amazon Elastic File System \(Amazon EFS\) file system is created in your AWS account\. This volume contains all of the home directories for all of the users in your AWS SSO domain\. This is where notebook files and data files are stored\. Users can’t directly access each other’s home directories\.  Don’t delete the Amazon EFS file system\.  If you do, your AWS SSO domain is longer functional, and all of your users will lose their work\. 