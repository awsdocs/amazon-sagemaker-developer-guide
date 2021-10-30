# Edge Manager Agent<a name="edge-device-fleet-about"></a>

The Edge Manager agent is an inference engine for your edge devices\. Use the agent to make predictions with models loaded onto your edge devices\. The agent also collects model metrics and captures data at specific intervals\. Sample data is stored in your Amazon S3 bucket\.

There are two methods of installing and deploying the Edge Manager agent onto your edge devices:

1. Download the agent as a binary from the Amazon S3 release bucket\. For more information, see [Download and Set Up Edge Manager Agent Manually](edge-device-fleet-manual.md)\.

1. Use the AWS IoT Greengrass V2 console or the AWS CLI to deploy `aws.greengrass.SageMakerEdgeManager`\. See [Create AWS IoT Greengrass V2 Components](edge-greengrass-custom-component.md)\.