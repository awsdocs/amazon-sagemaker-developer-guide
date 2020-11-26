# Configure model autoscaling with the console<a name="endpoint-auto-scaling-add-console"></a>

**To configure autoscaling for a model using the console**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Endpoints**\. 

1. Choose the endpoint that you want to configure\.

1. For **Endpoint runtime settings**, choose the model variant that you want to configure\.

1. For **Endpoint runtime settings**, choose **Configure autoscaling**\.

   The **Configure variant automatic scaling** page appears\.

1. For **Minimum capacity**, type the minimum number of instances that you want the scaling policy to maintain\. At least 1 instance is required\.

1. For **Maximum capacity**, type the maximum number of instances that you want the scaling policy to maintain\.

1. For the **target value**, type the average number of invocations per instance per minute for the model\. To determine this value, follow the guidelines in [Load testing](endpoint-scaling-loadtest.md)\.

   Application Auto Scaling adds or removes instances to keep the metric close to the value that you specify\.

1. For **Scale\-in cool down \(seconds\)** and **Scale\-out cool down \(seconds\)**, type the number seconds for each cool down period\. Assuming that the order in the list is based on either most important to less important of first applied to last applied\.

1. Select **Disable scale in** to prevent the scaling policy from deleting variant instances if you want to ensure that your variant scales out to address increased traffic, but are not concerned with removing instances to reduce costs when traffic decreases, disable scale\-in activities\.

   Scale\-out activities are always enabled so that the scaling policy can create endpoint instances as needed\.

1. Choose **Save**\.

This procedure registers a model as a scalable target with Application Auto Scaling\. When you register a model, Application Auto Scaling performs validation checks to ensure the following:
+ The model exists
+ The permissions are sufficient
+ You aren't registering a variant with an instance that is a burstable performance instance such as T2
**Note**  
SageMaker doesn't support autoscaling for burstable instances such as T2, because they already allow for increased capacity under increased workloads\. For information about burstable performance instances, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.