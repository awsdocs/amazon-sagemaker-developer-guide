# Check Status<a name="edge-device-fleet-check-status"></a>

Check your device or fleet is connected and sampling data\. Making periodic checks, manually or automatically, allows you to check your device or fleet is working properly\.

Use the [Amazon S3 console](https://console.aws.amazon.com/s3/) to interactively choose a fleet to check the status of\. You can also use the AWS SDK for Python \(Boto3\)\. The following describes different APIs from Boto3 you can use to check the status of your device or fleet\. Use the API that best fits your use case\.
+ **Check an individual device\.**

  To check the status of an individual device, use `DescribeDevice` API\. A list containing one or more models will be provided if a models have been deployed to the device\.

  ```
  sagemaker_client.describe_device(
      DeviceName="sample-device-1",
      DeviceFleetName="sample-fleet-name"
  )
  ```

  Running `DescribeDevice` returns:

  ```
  { "DeviceName": "sample-device".
    "Description": "this is a sample device",
    "DeviceFleetName": "sample-device-fleet",
    "IoTThingName": "SampleThing",
    "RegistrationTime": 1600977370,
    "LatestHeartbeat": 1600977370,
    "Models":[
          {
           "ModelName": "sample-model", 
           "ModelVersion": "1.1",
           "LatestSampleTime": 1600977370,
           "LatestInference": 1600977370 
          }
     ]
  }
  ```
+ **Check a fleet of devices\.**

  To check the status of the fleet, use the `GetDeviceFleetReport` API\. Provide the name of the device fleet to get a summary of the fleet\.

  ```
  sagemaer_client.get_device_fleet_report(
      DeviceFleetName="sample-fleet-name"
  )
  ```
+ **Check for a heart beat\.**

  Each device within a fleet is periodically generating a signal, or “heartbeat”\. The heart beat can be used to check that the device is communicating to Edge Manager\. If the time stamp of the last heartbeat is not being updated, then it might be that the device is failing\.

  Check the last heart beat with made by a device with `DescribeDevice` API\. Specify the name of the device and the fleet the edge device belongs to\.

  ```
  sagemaker_client.describe_device(
      DeviceName="sample-device-1",
      DeviceFleetName="sample-fleet-name"
  )
  ```