# Register a Device<a name="edge-device-fleet-register"></a>

**Important**  
Device registration is required to use any part of SageMaker Edge Manager\.

You can create a fleet programmatically with the AWS SDK for Python \(Boto3\) or through the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

## Register a Device \(Boto3\)<a name="edge-device-fleet-register-boto3"></a>

To register your device, first create and register an AWS IoT thing object and configure an IAM role\. SageMaker Edge Manager takes advantage of the AWS IoT Core services to facilitate the connection between the edge devices and the cloud\. You can take advantage of existing AWS IoT functionality after you set up your devices to work with Edge Manager\.

To connect your device to AWS IoT you need to create AWS IoT thing objects, create and register a client certificate with AWS IoT, and create and configure IAM role for your devices\.

See the [Getting Started Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/edge-manager-getting-started.html) for an in\-depth example or the [Explore AWS IoT Core services in hands\-on tutorial](https://docs.aws.amazon.com/iot/latest/developerguide/iot-gs-first-thing.html)\.

Use the `RegisterDevices` API to register your device\. Provide the name of the fleet of which you want the devices to be a part, as well as a name for the device\. You can optionally add a description to the device, tags, and AWS IoT thing name associated with the device\.

```
sagemaker_client.register_devices(
    DeviceFleetName="sample-fleet-name",
    Devices=[
        {          
            "DeviceName": "sample-device-1",
            "IotThingName": "sample-thing-name-1",
            "Description": "Device #1"
        }
     ],
     Tags=[
        {
            "Key": "string", 
            "Value" : "string"
         }
     ],
)
```

## Register a Device \(Console\)<a name="edge-device-fleet-register-console"></a>

You can register your device using the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. In the SageMaker console, choose **Edge Inference** and then choose choose **Edge devices**\.

1. Choose **Register devices**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/register-device-button.png)

1. In the **Device properties** section, enter the name of the fleet the device belongs to under the **Device fleet name** field\. Choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/register-devices-empty.png)

1. In the **Device source** section, add your devices one by one\. You must include a **Device Name** for each device in your fleet\. You can optionally provide a description \(in the **Description** field\) and an Internet of Things \(IoT\) object name \(in the **IoT name** field\)\. Choose **Submit** once you have added all your devices\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/register-devices-device-source.png)

   The **Devices** page displays the name of the device you have added, the fleet to which it belongs, when it was registered, the last heartbeat, and the description and AWS IoT name, if you provided one\.

   Choose a device to view the device’s details, including the device name, fleet, ARN, description, IoT Thing name, when the device was registered, and the last heartbeat\.