# View Endpoint Status<a name="model-dashboard-endpoints"></a>

If you want to use your trained model to perform inference on live data, you deploy your model to a real\-time endpoint\. To ensure appropriate latency of your predictions, you want to make sure the instances that host your model are running efficiently\. Model Dashboard’s endpoint monitoring feature displays real\-time information about your endpoint configuration and helps you track endpoint performance with metrics\. 

**Monitor settings**

The Model Dashboard links to existing SageMaker endpoint details pages which display real\-time graphs of metrics you can select in Amazon CloudWatch\. Within your dashboard, you can track these metrics as your endpoint is handling real\-time inference requests\. Some metrics you can select are the following:
+ `CpuUtilization`: The sum of each individual CPU core's utilization, with each ranging from 0%–100%\.
+ `MemoryUtilization`: The percentage of memory used by the containers on an instance, ranging from 0%–100%\.
+ `DiskUtilization`: The percentage of disk space used by the containers on an instance, ranging from 0%–100%\.

For the complete list of metrics you can view in real time, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\.

**Runtime settings**

Amazon SageMaker supports automatic scaling \(auto scaling\) for your hosted models\. Auto scaling dynamically adjusts the number of instances provisioned for a model in response to changes in your workload\. When the workload increases, auto scaling brings more instances online\. When the workload decreases, auto scaling removes unnecessary instances so that you don't pay for provisioned instances that you aren't using\. You can customize the following runtime settings in the Model Dashboard:
+ *Update weights*: Change the amount of workload assigned to each instance with numerical weighting\. For more information about instance weighting during auto scaling, see [Configure instance weighting for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-mixed-instances-groups-instance-weighting.html)\.
+ *Update instance count*: Change the number of total instances that can service your workload when it increases\.

For more information about endpoint runtime settings, see [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\.

**Endpoint configuration settings**

Endpoint configuration settings display the settings you specified when you created the endpoint\. These settings inform SageMaker which resources to provision for your endpoint\. Some settings included are the following:
+ *Data capture*: You can choose to capture information about your endpoint's inputs and outputs\. For example, you may want to sample incoming traffic to see if the results correlate to training data\. You can customize your sampling frequency, the format of the stored data, and Amazon S3 location of stored data\. For more information about setting up your data capture configuration, see [Capture data](model-monitor-data-capture.md)\.
+ *Production variants*: See the previous discussion in *Runtime settings*\.
+ *Async invocation configuration*: If your endpoint is asynchronous, this section includes the maximum number of concurrent requests sent by the SageMaker client to the model container, the Amazon S3 location of your success and failure notifications, and the output location of your endpoint outputs\. For more information about asynchronous outputs, see [Create, invoke, and update an Asynchronous Endpoint](async-inference-create-invoke-update-delete.md)\.
+ *Encryption key*: You can enter your encryption key if you want to encrypt your outputs\.

For more information about endpoint configuration settings, see [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\.

## View status and configuration for an endpoint<a name="model-dashboard-endpoint-view"></a>

**To view the status and configuration for a model’s endpoint, complete the following steps:**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Governance** in the left panel\.

1. Choose **Model Dashboard**\.

1. In the **Models** section of the Model Dashboard, select the model name of the endpoint you want to view\.

1. Select the endpoint name in the **Endpoints** section\.