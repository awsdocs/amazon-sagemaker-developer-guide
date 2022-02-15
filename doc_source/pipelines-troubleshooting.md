# Troubleshooting Amazon SageMaker Model Building Pipelines<a name="pipelines-troubleshooting"></a>

When using Amazon SageMaker Model Building Pipelines, you might run into issues for various reasons\. This topic provides information about common errors and how to resolve them\. 

 **Pipeline Definition Issues** 

Your pipeline definition might not be formatted correctly\. This can result in your execution failing or your job being inaccurate\. These errors can be caught when the pipeline is created or when an execution occurs\. If your definition doesn’t validate, SageMaker Pipelines returns an error message identifying the character where the JSON file is malformed\. To fix this problem, review the steps created using the SageMaker Python SDK for accuracy\. 

You can only include steps in a pipeline definition once\. Because of this, steps cannot exist as part of a condition step *and* a pipeline in the same pipeline\. 

 **Examining Pipeline Logs** 

You can view the status of your steps using the following command: 

```
execution.list_steps()
```

Each step includes the following information:
+ The ARN of the entity launched by the pipeline, such as SageMaker job ARN, model ARN, or model package ARN\. 
+ The failure reason includes a brief explanation of the step failure\.
+ If the step is a condition step, it includes whether the condition is evaluated to true or false\.  
+ If the execution reuses a previous job execution, the `CacheHit` lists the source execution\.  

You can also view the error messages and logs in the Amazon SageMaker Studio interface\. For information about how to see the logs in Studio, see [View a Pipeline Execution](pipelines-studio-view-execution.md)\.

 **Missing Permissions** 

Correct permissions are required for the role that creates the pipeline execution, and the steps that create each of the jobs in your pipeline execution\. Without these permissions, you may not be able to submit your pipeline execution or run your SageMaker jobs as expected\. To ensure that your permissions are properly set up, see [Access Management](build-and-manage-access.md)\. 

 **Job Execution Errors ** 

You may run into issues when executing your steps because of issues in the scripts that define the functionality of your SageMaker jobs\. Each job has a set of CloudWatch logs\. To view these logs from Studio, see [View a Pipeline Execution](pipelines-studio-view-execution.md)\. For information about using CloudWatch logs with SageMaker, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\. 

 **Property File Errors** 

You may have issues when incorrectly implementing property files with your pipeline\. To ensure that your implementation of property files works as expected, see [Property Files and `JsonGet`](build-and-manage-propertyfile.md)\. 