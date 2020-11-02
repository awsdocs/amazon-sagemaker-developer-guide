# Track Worker Performance<a name="workteam-private-tracking"></a>

 Amazon SageMaker Ground Truth logs worker events to Amazon CloudWatch, such as when a worker starts or submits a task\. Use Amazon CloudWatch metrics to measure and track throughput across a team or for individual workers\. 

**Important**  
Worker event tracking is not available for Amazon Augmented AI human review workflows\. 

## Enable Tracking<a name="workteam-private-tracking-setup"></a>

 During the set\-up process for a new work team, the permissions for Amazon CloudWatch logging of worker events are created\. Since this feature was added in August 2019, work teams created prior to that may not have the correct permissions\. If all of your work teams were created before August 2019, create a new work team\. It does not need any members and may be deleted after creation, but by creating it, you establish the permissions and apply them to all of your work teams, regardless of when they were created\. 

## Examine Logs<a name="workteam-private-tracking-logs"></a>

 After tracking is enabled, the activity of your workers is logged\. Open the Amazon CloudWatch console and choose **Logs** in the navigation pane\. You should see a log group named **/aws/sagemaker/groundtruth/WorkerActivity**\. 

Each completed task is represented by a log entry, which contains information about the worker, their team, the job, when the task was accepted, and when it was submitted\.

**Example Log entry**  

```
{
  "worker_id": "cd449a289e129409",
  "cognito_user_pool_id": "us-east-2_IpicJXXXX",
  "cognito_sub_id": "d6947aeb-0650-447a-ab5d-894db61017fd",
  "task_accepted_time": "Wed Aug 14 16:00:59 UTC 2019",
  "task_submitted_time": "Wed Aug 14 16:01:04 UTC 2019",
  "task_returned_time": "",
  "workteam_arn": "arn:aws:sagemaker:us-east-2:############:workteam/private-crowd/Sample-labeling-team",
  "labeling_job_arn": "arn:aws:sagemaker:us-east-2:############:labeling-job/metrics-demo",
  "work_requester_account_id": "############",
  "job_reference_code": "############",
  "job_type": "Private",
  "event_type": "TasksSubmitted",
  "event_timestamp": "1565798464"
}
```

A useful data point in each event is the `cognito_sub_id`\. You can match that to an individual worker\.

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Under the **Ground Truth** section, choose **Workforces**\.

1. Choose **Private**\.

1. Choose the name of a team in the **Private teams** section\.

1. In the **Team summary** section, choose the user group identified under **Amazon Cognito user group**\. That will take you to the group in the Amazon Cognito console\.

1. The **Group** page lists the users in the group\. Choose any user's link in the **Username** column to see more information about the user, including a unique **sub** ID\.

To get information about all of the team's members, use the [ListUsers](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html) action \([examples](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-manage-user-accounts.html#cognito-user-pools-searching-for-users-listusers-api-examples)\) in the Amazon Cognito API\.

## Use Log Metrics<a name="workteam-private-tracking-metrics"></a>

If you don't want to write your own scripts to process and visualize the raw log information, Amazon CloudWatch metrics provide insights into worker activity for you\.

**To view metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose the **AWS/SageMaker/Workteam** name space, then explore the [available metrics](monitoring-cloudwatch.md)\. For example, selecting the **Workflow, Workteam** metrics lets you calculate the average time per submitted task for a specific labeling job\.

For more information, see [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)\.