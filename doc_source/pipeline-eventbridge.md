# Amazon EventBridge Integration<a name="pipeline-eventbridge"></a>

You can schedule your Amazon SageMaker Model Building Pipelines executions using [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)\. Amazon SageMaker Model Building Pipelines is supported as a target in [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html)\. This allows you to trigger the execution of your model building pipeline based on any event in your event bus\. EventBridge enables you to automate your pipeline executions and respond automatically to events such as training job or endpoint status changes\. Events include a new file being uploaded to your Amazon S3 bucket, a change in status of your Amazon SageMaker endpoint due to drift, and *Amazon Simple Notification Service* \(SNS\) topics\.

The following SageMaker Pipelines actions can be automatically triggered:  
+  `StartPipelineExecution` 

For more information on scheduling SageMaker jobs, see [Automating SageMaker with Amazon EventBridge\.](https://docs.aws.amazon.com/sagemaker/latest/dg/automating-sagemaker-with-eventbridge.html) 

## Schedule a Pipeline with Amazon EventBridge<a name="pipeline-eventbridge-schedule"></a>

To start a pipeline execution with Amazon CloudWatch Events, you must create an EventBridge [rule](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_Rule.html)\. When you create a rule for events, you specify a target action to take when EventBridge receives an event that matches the rule\. When an event matches the rule, EventBridge sends the event to the specified target and triggers the action defined in the rule\. 

 The following tutorials show how to schedule a pipeline execution with EventBridge using the EventBridge console or the AWS CLI\.  

### Prerequisites<a name="pipeline-eventbridge-schedule-prerequisites"></a>
+  A role that can be assumed by EventBridge with the `SageMaker::StartPipelineExecution` permission\. This role can be created automatically if you create a rule from the EventBridge console, otherwise you need to create this role yourself\. For information on creating a SageMaker role, see [SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.
+  An Amazon SageMaker Pipeline to schedule\. To create an Amazon SageMaker Pipeline, see [Define a Pipeline](https://docs.aws.amazon.com/sagemaker/latest/dg/define-pipeline.html)\.

### Create an EventBridge rule using the EventBridge console<a name="pipeline-eventbridge-schedule-console"></a>

 The following procedure shows how to create an EventBridge rule using the EventBridge console\.  

1.  Navigate to the [EventBridge console](https://console.aws.amazon.com/events)\. 

1. Select `Rules` on the left hand side\. 

1.  Select `Create Rule`\. 

1. Enter a name and description for your rule\.

1.  Select how you want this rule to be triggered\. You have the following choices for your rule: 
   +  **Event pattern**: Your rule is triggered when an event matching the pattern occurs\. You can choose a pre\-defined pattern that matches a certain type of event, or you can create a custom pattern\. If you select a pre\-defined pattern, you can edit the pattern to customize it\. For more information on Event patterns, see [Event Patterns in CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html)\. 
   +  **Schedule**: Your rule is triggered regularly on a specified schedule\. You can use a fixed\-rate schedule that triggers regularly for a specified number of minutes, hour, or weeks\. You can also use a [cron expression](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) to create a more fine\-grained schedule, such as “the first Monday of each month at 8am\.” Schedule is not supported on a custom or partner event bus\. 

1. Select your desired Event bus\. 

1. Select the target\(s\) to invoke when an event matches your event pattern or when the schedule is triggered\. You can add up to 5 targets per rule\. Select `SageMaker Pipeline` in the target drop\-down\. 

1. Select the pipeline you want to trigger from the pipeline drop\-down\. 

1. Add parameters to pass to your pipeline execution using a name and value pair\. Parameter values can be static or dynamic\. For more information on Amazon SageMaker Pipeline parameters, see [AWS::Events::Rule SagemakerPipelineParameters](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-events-rule-sagemakerpipelineparameters.html)\.
   + Static values are passed to the pipeline execution every time the pipeline is triggered\. For example, if `{"Name": "Instance_type", "Value": "ml.4xlarge"}` is specified in the parameter list, then it will be passed as a parameter in `StartPipelineExecutionRequest` every time EventBridge triggers the pipeline\. 
   + Dynamic values are specified using a json path\. EventBridge parses the value from an event payload, then passes it to the pipeline execution\. For example: *`$.detail.param.value`* 

1. Select the role to use for this rule\. You can either use an existing role or create a new one\. 

1. \(Optional\) Add tags\. 

1. Select `Create` to finalize your rule\. 

 Your rule is now in effect and ready to trigger your pipeline executions\. 

### Create an EventBridge rule using the [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/events/index.html)<a name="pipeline-eventbridge-schedule-cli"></a>

 The following procedure shows how to create an EventBridge rule using the AWS CLI\. 

1. Create a rule to be triggered\. When creating an EventBridge rule using the AWS CLI, you have two options for how your rule is triggered, event pattern and schedule\.
   +  **Event pattern**: Your rule is triggered when an event matching the pattern occurs\. You can choose a pre\-defined pattern that matches a certain type of event, or you can create a custom pattern\. If you select a pre\-defined pattern, you can edit the pattern to customize it\.  You can create a rule with event pattern using the following command: 

     ```
     aws events put-rule --name <RULE_NAME> ----event-pattern <YOUR_EVENT_PATTERN> --description <RULE_DESCRIPTION> --role-arn <ROLE_TO_EXECUTE_PIPELINE> --tags <TAGS>
     ```
   +  **Schedule**: Your rule is triggered regularly on a specified schedule\. You can use a fixed\-rate schedule that triggers regularly for a specified number of minutes, hour, or weeks\. You can also use a cron expression to create a more fine\-grained schedule, such as “the first Monday of each month at 8am\.” Schedule is not supported on a custom or partner event bus\. You can create a rule with schedule using the following command: 

     ```
     aws events put-rule --name <RULE_NAME> --schedule-expression <YOUR_CRON_EXPRESSION> --description <RULE_DESCRIPTION> --role-arn <ROLE_TO_EXECUTE_PIPELINE> --tags <TAGS>
     ```

1.  Add target\(s\) to invoke when an event matches your event pattern or when the schedule is triggered\. You can add up to 5 targets per rule\.  For each target, you must specify:  
   +  ARN: The resource ARN of your pipeline\. 
   +  Role ARN: The ARN of the role EventBridge should assume to execute the pipeline\. 
   +  Parameters:  Amazon SageMaker pipeline parameters to pass\. 

1.  Run the following command to pass a Amazon SageMaker pipeline as a target to your rule using [put\-targets](https://docs.aws.amazon.com/cli/latest/reference/events/put-targets.html) : 

   ```
   aws events put-targets --rule <RULE_NAME> --event-bus-name <EVENT_BUS_NAME> --targets "[{\"Id\": <ID>, \"Arn\": <RESOURCE_ARN>, \"RoleArn\": <ROLE_ARN>, \"SageMakerPipelineParameter\": { \"SageMakerParameterList\": [{\"Name\": <NAME>, \"Value\": <VALUE>}]} }]"] 
   ```