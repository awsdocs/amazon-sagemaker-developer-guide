# Step 5: Monitoring Your Labeling Job<a name="sms-getting-started-step5"></a>

After you create your labeling job, you see a list of all the jobs that you have created\. You can use this list to monitor that status of your labeling jobs\. The list has the following fields:
+ **Name** – The name that you assigned the job when you created it\.
+ **Status** – The completion status of the job\. The status can be one of Complete, Failed, In progress, or Stopped\.
+ **Labeled objects/total** – Shows the total number of objects in the labeling job and how many of them have been labeled\.
+ **Creation time** – The date and time that you created the job\.

You can also clone, chain, or stop a job\. Select a job and then select one of the following from the **Actions** menu:
+ **Clone** – Creates a new labeling job with the configuration copied from the selected job\. You can clone a job when you want to change to the job and run it again\. For example, you can clone a job that was sent to a private workforce so that you can send it to the Amazon Mechanical Turk workforce\. Or you can clone a job to rerun it against a new dataset stored in the same location as the original job\.
+ **Chain** – Creates a new labeling job that can build upon the data and models \(if any\) of a stopped, failed, or completed job\. For more information about the use cases and how to use it, see [Chaining Labeling Jobs](sms-reusing-data.md)\.
+ **Stop** – Stops a running job\. You cannot restart a stopped job\. You can clone a job to start over or chain the job to continue from where it left off\. Labels for any already labeled objects are written to the output file location\. For more information, see [Output Data](sms-data-output.md)\.