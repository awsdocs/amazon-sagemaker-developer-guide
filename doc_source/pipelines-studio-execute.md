# Execute a Pipeline<a name="pipelines-studio-execute"></a>

This procedure shows you how to execute a pipeline\. For information on how to view a list of pipeline executions, see [View a Pipeline](pipelines-studio-list-pipelines.md)\.

**To start a pipeline execution**

1. From the **Executions** or **Graph** tab in the execution list, choose **Start an execution**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-list.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-start.png)

1. Enter or update the following required information:
   + **Name** – Must be unique to your account in the AWS Region\.
   + **ProcessingInstanceType** – The instance type for processing\.
   + **ProcessingInstanceCount** – The number of instances to use for processing\.
   + **TrainingInstanceType** – The instance type for training\.
   + **ModelApprovalStatus** – For your convenience\.
   + **InputData** – The S3 URI of the input data\.

1. Choose **Submit**\.
+ To see details of the execution or to stop the execution, choose **View details** on the status banner\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-details.png)
+ To stop the execution, choose **Stop** on the status banner\.
+ To resume the execution from where it was stopped, choose **Resume** on the status banner\.

**Note**  
If your pipeline fails, the status banner will show **Failed** status\. After troubleshooting the failed step, choose **Resume** on the status banner to resume running the pipeline from that step\.

For a list of registered models, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.