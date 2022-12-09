# View notebook jobs<a name="view-notebook-jobs"></a>

The **Notebook Jobs** tab shows a history of your on\-demand jobs and all the jobs that run from the job definitions you created\. This tab opens after you create an on\-demand job, or you can just view this tab yourself to see a history of past and current jobs\. If you select the **Job name** for any job, you can view details for a single job in its **Job Detail** page\. For more information about the **Job Detail** page, see the following section [View a single job](#view-jobs-detail-notebook-auto-run)\.

The **Notebook Jobs** tab includes the following information for each job:
+ **Output files**: Displays the availability of output files\. This column can contain one of the following:
  + A download icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_download.png) \): The output notebook and log are available for download; choose this button to download them\. Note that a failed job can still generate output files if the failure occurred after the files were created\. In this case, it is helpful to view the output notebook to identify the failure point\.
  + Links to the **Notebook** and **Output log**: The notebook and output log are downloaded\. Choose the links to view their contents\.
  + \(blank\): The job was stopped by the user, or a failure occurred in the job run before it could generate output files\. For example, network failures could prevent the job from starting\.

  The output notebook is the result of running all cells in the notebook, and also incorporates any new or overriding parameters or environment variables you included\. The output log captures the details of the job run to help you troubleshoot failed jobs\.
+ **Created at**: The time the on\-demand job or scheduled job was created\.
+ **Status**: The current status of the job, which is one of the following values:
  + **In progress**: The job is running
  + **Failed**: The job failed from configuration or notebook logic errors
  + **Stopped**: The job was stopped by the user
  + **Completed**: The job completed
+ **Actions**: This column provides shortcuts to help you stop or remove any job directly in the interface\.

## View a single job<a name="view-jobs-detail-notebook-auto-run"></a>

From the **Notebook Jobs** tab, you can select a job name to view the **Job Detail** page for a specific job\. The **Job Detail** page includes all the details you provided in the **Create Job** form\. Use this page to confirm the settings you specified when you created the job definition\. 

In addition, you can access shortcuts to help you perform the following actions in the page itself:
+ **Delete Job**: Remove the job from the **Notebook Jobs** tab\.
+ **Stop Job**: Stop your running job\.