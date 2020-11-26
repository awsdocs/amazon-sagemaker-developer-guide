# Access Notebook Instances<a name="howitworks-access-ws"></a>

To access your Amazon SageMaker notebook instances, choose one of the following options: 
+ Use the console\.

  Choose **Notebook instances**\. The console displays a list of notebook instances in your account\. To open a notebook instance with a standard Jupyter interface, choose **Open Jupyter** for that instance\. To open a notebook instance with a JupyterLab interface, choose **Open JupyterLab** for that instance\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ws-notebook-10.png)

  The console uses your sign\-in credentials to send a [  `CreatePresignedNotebookInstanceUrl`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedNotebookInstanceUrl.html) API request to SageMaker\. SageMaker returns the URL for your notebook instance, and the console opens the URL in another browser tab and displays the Jupyter notebook dashboard\. 
**Note**  
The URL that you get from a call to [  `CreatePresignedNotebookInstanceUrl`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedNotebookInstanceUrl.html) is valid only for 5 minutes\. If you try to use the URL after the 5\-minute limit expires, you are directed to the AWS Management Console sign\-in page\.
+ Use the API\.

  To get the URL for the notebook instance, call the [ `CreatePresignedNotebookInstanceUrl`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedNotebookInstanceUrl.html) API and use the URL that the API returns to open the notebook instance\.

Use the Jupyter notebook dashboard to create and manage notebooks and to write code\. For more information about Jupyter notebooks, see [http://jupyter\.org/documentation\.html](http://jupyter.org/documentation.html)\.