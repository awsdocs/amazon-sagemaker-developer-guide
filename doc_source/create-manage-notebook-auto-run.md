# Create and manage your scheduled notebook jobs<a name="create-manage-notebook-auto-run"></a>

The SageMaker Studio notebook scheduling feature gives you the tools to create and manage your non\-interactive notebook jobs\. You can create jobs, view the jobs you created, and pause, stop, or resume existing jobs\. You can also modify notebook schedules\.

When you create your scheduled notebook job, Studio automatically populates the form with default options to help you get started quickly\. For example, Studio generates resources such as Amazon S3 buckets needed to schedule your notebook job if you don’t specify your own\. Studio also infers details related to your infrastructure, such as your kernel, image, role ARN, and security groups\. At a minimum, you can submit an on\-demand job without setting any options or a \(scheduled\) notebook job definition supplying just the time\-specific schedule information\. However, you can customize other fields if your scheduled job requires specialized settings\. For details about how to schedule a notebook job, see [Schedule a notebook job](create-notebook-auto-run.md)\.

The scheduled notebook dashboards help organize the job definitions that you schedule, and also keep track of the actual jobs that run from your job definitions\. There are two important concepts to understand when scheduling notebook jobs: job definitions and job runs\. *Job definitions* are schedules you set to run specific notebooks\. For example, you can create a job definition that runs notebook XYZ\.ipynb every Wednesday\. This job definition launches the actual *job runs* which occur this coming Wednesday, next Wednesday, the Wednesday after that, and so on\. 

The Studio interface provides two main tabs that help you track your existing job definitions and job runs:
+ **Notebook Jobs** tab: This tab displays a list of all your job runs from your on\-demand jobs and job definitions\. From this tab, you can directly access the details for a single job run\. For example, you can view a single job run that occurred two Wednesdays ago\.
+ **Notebook Job Definitions** tab: This tab displays a list of all your job definitions\. From this tab, you can directly access the details for a single job definition\. For example, you can view the schedule you created to run XYZ\.ipynb every Wednesday\.

For details about the **Notebook Jobs** tab, see [View notebook jobs](view-notebook-jobs.md)\.

For details about the **Notebook Job Definitions** tab, see [View notebook job definitions](view-def-detail-notebook-auto-run.md)\.