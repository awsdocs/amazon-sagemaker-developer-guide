# Auto\-Rollback Configuration and Monitoring<a name="deployment-guardrails-configuration"></a>

Amazon CloudWatch alarms are a prerequisite for using baking periods in deployment guardrails\. You can only use the auto\-rollback functionality in deployment guardrails if you set up CloudWatch alarms that can monitor an endpoint\. If any of your alarms trip during the specified monitoring period, SageMaker initiates a complete rollback to the old endpoint to protect your application\. If you do not have any CloudWatch alarms set up to monitor your endpoint, then the auto\-rollback functionality does not work during your deployment\.

To learn more about Amazon CloudWatch, see [What is Amazon CloudWatch?](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *Amazon CloudWatch User Guide*\.

## Alarm Examples<a name="deployment-guardrails-configuration-alarm-examples"></a>

To help you get started, we provide the following examples to demonstrate the capabilities of CloudWatch alarms\. In addition to using or modifying the following examples, you can create your own alarms and configure the alarms to monitor various metrics on the specified fleets for a certain period of time\. To see more SageMaker metrics and dimensions you can add to your alarms, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\.

**Topics**
+ [Monitor invocation errors on both old and new fleets](#deployment-guardrails-configuration-alarm-examples-errors-both)
+ [Monitor model latency on the new fleet](#deployment-guardrails-configuration-alarm-examples-latency-new)

### Monitor invocation errors on both old and new fleets<a name="deployment-guardrails-configuration-alarm-examples-errors-both"></a>

The following CloudWatch alarm monitors an endpoint's average error rate\. You can use this alarm with any deployment guardrails traffic shifting type to provide overall monitoring on both the old and new fleets\. If the alarm trips, then SageMaker initiates a rollback to the old fleet\.

Invocation errors coming from both the old fleet and new fleet contribute to the average error rate\. If the average error rate exceeds the specified threshold, then the alarm trips\. This particular example monitors the 4xx errors \(client errors\) on both the old and new fleets for the duration of a deployment\. You can also monitor the 5xx errors \(server errors\) by using the metric `Invocation5XXErrors`\.

**Note**  
For this alarm type, if your old fleet trips the alarm during the deployment, SageMaker terminates your deployment\. Therefore, if your current production fleet already causes errors, consider using or modifying one of the following examples that only monitors the new fleet for errors\.

```
#Applied deployment type: all types
{
    "AlarmName": "EndToEndDeploymentHighErrorRateAlarm",
    "AlarmDescription": "Monitors the error rate of 4xx errors",
    "MetricName": "Invocation4XXErrors",
    "Namespace": "AWS/SageMaker",
    "Statistic": "Average",
    "Dimensions": [
        {
            "Name": "EndpointName",
            "Value": <your-endpoint-name>
        },
        {
            "Name": "VariantName",
            "Value": "AllTraffic"
        }
    ],
    "Period": 600,
    "EvaluationPeriods": 2,
    "Threshold": 1,
    "ComparisonOperator": "GreaterThanThreshold",
    "TreatMissingData": "notBreaching"
}
```

In the previous example, note the values for the following fields:
+ For `AlarmName` and `AlarmDescription`, enter a name and description you choose for the alarm\.
+ For `MetricName`, use the value `Invocation4XXErrors` to monitor for 4xx errors on the endpoint
+ For `Namespace`, use the value `AWS/SageMaker`\. You can also specify your own custom metric, if applicable\.
+ For `Statistic`, use `Average`\. This means that the alarm takes the average error rate over the evaluation periods when calculating whether the error rate has exceeded the threshold\.
+ For the dimension `EndpointName`, use the name of the endpoint you are updating as the value\.
+ For the dimension `VariantName`, use the value `AllTraffic` to specify all endpoint traffic\.
+ For `Period`, use `600`\. This sets the alarm’s evaluation periods to 10 minutes long\.
+ For `EvaluationPeriods`, use `2`\. This value tells the alarm to consider the two most recent evaluation periods when determining the alarm status\.

### Monitor model latency on the new fleet<a name="deployment-guardrails-configuration-alarm-examples-latency-new"></a>

The following CloudWatch alarm example monitors the new fleet’s model latency during your deployment\. You can use this alarm to monitor only the new fleet and exclude the old fleet\. The alarm lasts for the entire deployment\. This example gives you comprehensive, end\-to\-end monitoring of the new fleet and initiates a rollback to the old fleet if the new fleet has any response time issues\.

CloudWatch publishes the metrics with the dimension `EndpointConfigName:{New-Ep-Config}` after the new fleet starts receiving traffic, and these metrics last even after the deployment is complete\.

You can use the following alarm example with any deployment type\.

```
#Applied deployment type: all types
{
    "AlarmName": "NewEndpointConfigVersionHighModelLatencyAlarm",
    "AlarmDescription": "Monitors the model latency on new fleet",
    "MetricName": "ModelLatency",
    "Namespace": "AWS/SageMaker",
    "Statistic": "Average",
    "Dimensions": [
        {
            "Name": "EndpointName",
            "Value": <your-endpoint-name>
        },
        {
            "Name": "VariantName",
            "Value": "AllTraffic"
        },
        {
            "Name": "EndpointConfigName",
            "Value": <your-config-name>
    ],
    "Period": 300,
    "EvaluationPeriods": 2,
    "Threshold": 100000, # 100ms
    "ComparisonOperator": "GreaterThanThreshold",
    "TreatMissingData": "notBreaching"
}
```

In the previous example, note the values for the following fields:
+ For `MetricName`, use the value `ModelLatency` to monitor the model’s response time\.
+ For `Namespace`, use the value `AWS/SageMaker`\. You can also specify your own custom metric, if applicable\.
+ For the dimension `EndpointName`, use the name of the endpoint you are updating as the value\.
+ For the dimension `VariantName`, use the value `AllTraffic` to specify all endpoint traffic\.
+ For the dimension `EndpointConfigName`, the value should refer to the endpoint configuration name for your new or updated endpoint\.

**Note**  
If you want to monitor your old fleet instead of the new fleet, you can change the dimension `EndpointConfigName` to specify the name of your old fleet’s configuration\.