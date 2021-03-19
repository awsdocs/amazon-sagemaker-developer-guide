# Create Actions on Rules Using Amazon CloudWatch and AWS Lambda<a name="debugger-cloudwatch-lambda"></a>

Amazon CloudWatch collects Amazon SageMaker model training job logs and Amazon SageMaker Debugger rule processing job logs\. Configure Debugger with Amazon CloudWatch Events and AWS Lambda to take action based on Debugger rule evaluation status\.

## CloudWatch Logs for Debugger Rules and Training Jobs<a name="debugger-cloudwatch-metric"></a>

**To find training job logs and Debugger rule job logs**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane under the **Log** node, choose **Log Groups**\.

1. In the log groups list, do the following:
   + Choose **/aws/sagemaker/TrainingJobs** for training job logs\.
   + Choose **/aws/sagemaker/ProcessingJobs** for Debugger rule job logs\.

You can use the training and Debugger rule job status in the CloudWatch logs to take further actions when there are training issues\.

For more information about monitoring training jobs using CloudWatch, see [Monitor Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-overview.html)\.

## Set Up Debugger for Automated Training Job Termination Using CloudWatch and Lambda<a name="debugger-stop-training"></a>

The Debugger rules monitor training job status, and a CloudWatch Events rule watches the Debugger rule training job evaluation status\.

### Step 1: Create a Lambda Function<a name="debugger-lambda-function-create"></a>

**To create a Lambda function**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the left navigation pane, choose **Functions** and then choose **Create function**\.

1. On the **Create function** page, choose **Author from scratch** option\.

1. In the **Basic information** section, enter a **Function name** \(for example, **debugger\-rule\-stop\-training\-job**\)\.

1. For **Runtime**, choose **Python 3\.7**\.

1. For **Permissions**, expand the drop down option, and choose **Choose or create an execution role**\.

1. For **Execution role**, choose **Use an existing role** and choose the IAM role that you used for your SageMaker Notebook instance or SageMaker Studio\. 
**Note**  
Make sure you use the same execution role for the training environment, otherwise the Lambda function won't properly react to the Debugger rule status changes\. If you are unsure which execution role to choose, run the following code in a Jupyter notebook cell to retrieve the execution role output:  

   ```
   import sagemaker
   sagemaker.get_execution_role()
   ```

1. At the bottom of the page, choose **Create function**\.

The following figure shows an example of the **Create function** page with the input fields and selections completed\.

![\[Create Function page.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-lambda-create.png)

### Step 2: Configure the Lambda function<a name="debugger-lambda-function-configure"></a>

**To configure the Lambda function**

1. In the **Function code** section of the configuration page, paste the following Python script in the Lambda code editor pane\. The `lambda_handler` function monitors the Debugger rule evaluation status collected by CloudWatch and triggers the `StopTrainingJob` API operation\. The AWS SDK for Python \(Boto3\) `client` for SageMaker provides a high\-level method, `stop_training_job`, which triggers the `StopTrainingJob` API operation\.

   ```
   import json
       import boto3
       import logging
       
       def lambda_handler(event, context):
           training_job_name = event.get("detail").get("TrainingJobName")
           eval_statuses = event.get("detail").get("DebugRuleEvaluationStatuses", None)
       
           if eval_statuses is None or len(eval_statuses) == 0:
               logging.info("Couldn't find any debug rule statuses, skipping...")
               return {
                   'statusCode': 200,
                   'body': json.dumps('Nothing to do')
               }
       
           client = boto3.client('sagemaker')
       
           for status in eval_statuses:
               if status.get("RuleEvaluationStatus") == "IssuesFound":
                   logging.info(
                       'Evaluation of rule configuration {} resulted in "IssuesFound". '
                       'Attempting to stop training job {}'.format(
                           status.get("RuleConfigurationName"), training_job_name
                       )
                   )
                   try:
                       client.stop_training_job(
                           TrainingJobName=training_job_name
                       )
                   except Exception as e:
                       logging.error(
                           "Encountered error while trying to "
                           "stop training job {}: {}".format(
                               training_job_name, str(e)
                           )
                       )
                       raise e
           return None
   ```

   For more information about the Lambda code editor interface, see [Creating functions using the AWS Lambda console editor](https://docs.aws.amazon.com/lambda/latest/dg/code-editor.html)\.

1. Skip all other settings and choose **Save** at the top of the configuration page\.

### Step 3: Create a CloudWatch Events Rule and Link to the Lambda Function for Debugger<a name="debugger-cloudwatch-events"></a>

**To create a CloudWatch Events rule and link to the Lambda function for Debugger**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Rules** under the **Events** node\.

1. Choose **Create rule**\.

1. In the **Event Source** section of the **Step 1: Create rule** page, choose **SageMaker** for **Service Name**, and choose **SageMaker Training Job State Change** for **Event Type**\. The Event Pattern Preview should look like the following example JSON strings: 

   ```
   {
       "source": [
           "aws.sagemaker"
       ],
       "detail-type": [
           "SageMaker Training Job State Change"
       ]
   }
   ```

1. In the **Targets** section, choose **Add target\***, and choose the **debugger\-rule\-stop\-training\-job** Lambda function that you created\. This step links the CloudWatch Events rule with the Lambda function\.

1. Choose **Configure details** and go to the **Step 2: Configure rule details** page\.

1. Specify the CloudWatch rule definition name\. For example, **debugger\-cw\-event\-rule**\.

1. Choose **Create rule** to finish\.

1. Go back to the Lambda function configuration page and refresh the page\. Confirm that it's configured correctly in the **Designer** panel\. The CloudWatch Events rule should be registered as a trigger for the Lambda function\. The configuration design should look like the following example:  
<a name="lambda-designer-example"></a>![\[Designer panel for the CloudWatch configuration.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-lambda-designer.png)

## Run Example Notebooks to Test Automated Training Job Termination<a name="debugger-test-stop-training"></a>

You can run the following example notebooks, which are prepared for experimenting with stopping a training job using Debugger's built\-in rules\.
+ [Amazon SageMaker Debugger \- Reacting to CloudWatch Events from Debugger rules](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow_action_on_rule/tf-mnist-stop-training-job.ipynb)

  This example notebook runs a training job that has a vanishing gradient issue\. The Debugger [VanishingGradient](debugger-built-in-rules.md#vanishing-gradient) built\-in rule is used while constructing the SageMaker TensorFlow estimator\. When the Debugger rule detects the issue, the training job is terminated\.
+ [Detect stalled training and stop training job using Debugger rule](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow_action_on_rule/detect_stalled_training_job_and_stop.ipynb)

  This example notebook runs a training script with a code line that forces it to sleep for 10 minutes\. The Debugger [StalledTrainingRule](debugger-built-in-rules.md#stalled-training) built\-in rule invokes issues and stops the training job\.

## Disable the CloudWatch Events Rule to Stop Using the Automated Training Job Termination<a name="debugger-disable-cw"></a>

If you want to disable the automated training job termination, you need to disable the CloudWatch Events rule\. In the Lambda **Designer** panel, choose the **EventBridge \(CloudWatch Events\)** block linked to the Lambda function\. This shows an **EventBridge** panel below the **Designer** panel \(for example, see the previous screen shot\)\. Select the check box next to **EventBridge \(CloudWatch Events\): debugger\-cw\-event\-rule**, and then choose **Disable**\. If you want to use the automated termination functionality later, you can enable the CloudWatch Events rule again\.