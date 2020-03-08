# Use Amazon SageMaker Notebooks<a name="notebooks"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

 Amazon SageMaker Studio notebooks are collaborative notebooks that are built into Amazon SageMaker Studio that you can launch quickly\. You can access your notebooks without setting up compute instances and file storage so you can get started fast\. You pay only for the resources consumed when you run the notebooks\. 

 After your organization is set up with Amazon SageMaker Studio, your organization’s data scientists, developers, and other SageMaker users can start working on notebooks within seconds without having to provision any compute resources\.  Each of your users is automatically assigned their own instance to execute the code of the notebooks, but they can also have multiple instances for different notebooks\. Amazon SageMaker Studio notebooks provides persistent storage for your users’ notebooks, which enables them to view and share notebooks even if the instances are shut down\. 

 You can share your notebooks with others in your organization, so that they can easily reproduce your results and collaborate while building models and exploring your data\. Dependencies for your notebook are included in the notebooks’ environment settings, so sharing is seamless\. Sharing publishes a reproducible snapshot of your notebook environments through secure sharable URLs\. 

 To get started, you or your organization’s administrator need to complete the Amazon SageMaker Studio setup for the team\. There are two modes for authentication: AWS Single Sign\-On \(AWS SSO\) and AWS Identity and Access Management \(IAM\) \. When choosing the appropriate method for your team, you must carefully consider the advantages of each method\. Switching to another authentication method after you set up the domain requires a manual migration of the team’s notebooks so you want to make sure you choose the appropriate method for your organization at the start\. For more information, see the [setup documentation](gs-set-up.html)\. 

**Note**  
Because Amazon SageMaker Studio Notebooks is in preview, visual elements of Amazon SageMaker Studio might be impacted\.