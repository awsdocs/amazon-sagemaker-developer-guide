# Check Device and Fleet<a name="edge-getting-started-step6"></a>

Check your device or fleet is connected and sampling data\. Making periodic checks, manually or automatically, allows you to check your device or fleet is working properly\.
+ **Check a single device in your fleet\.**

  Check that your device is working\. You need to provide the name of the fleet to which the device belongs and the unique device identifier\.

  ```
  sagemaker_client.describe_device(
      DeviceName="sample-device-1",
      DeviceFleetName=device_fleet_name
  )
  ```

  For the given model, you can see the name, model version, latest sample time, and when the last inference was made\.

  ```
  { "DeviceName": "sample-device",
    "DeviceFleetName": "sample-device-fleet",
    "IoTThingName": "sample-thing-name-1",
    "RegistrationTime": 1600977370,
    "LatestHeartbeat": 1600977370,
    "Models":[
      {
          "ModelName": "mobilenet_v2.tar.gz", 
          "ModelVersion": "1.1",
          "LatestSampleTime": 1600977370,
          "LatestInference": 1600977370 
      }
    ]
  }
  ```

  The timestamp provided by `LastetHeartbeat` indicates the last signal that was received from the device\. `LatestSampleTime` and `LatestInference` describe the time stamp of the last data sample and inference, respectively\.
+ **Check your fleet\.**

  Check that your fleet is working with `GetDeviceFleetReport`\. Provide the name of the fleet the device belongs to\.

  ```
  sagemaer_client.get_device_fleet_report(
      DeviceFleetName=device_fleet_name
  )
  ```

  For a given model, you can see the name, model version, latest sample time, and when the last inference was made, along with the Amazon S3 bucket URI where the data samples are stored\.

  ```
  # Sample output
  {"DeviceFleetName": "sample-device-fleet",
   "DeviceFleetArn": "arn:aws:sagemaker:us-west-2:9999999999:device-fleet/sample-fleet-name",
   "OutputConfig": {
                "S3OutputLocation": "s3://fleet-bucket/package_output",
    },
   "AgentVersions":[{"Version": "1.1", "AgentCount": 2}]}
   "DeviceStats": {"Connected": 2, "Registered": 2}, 
   "Models":[{
              "ModelName": "sample-model", 
              "ModelVersion": "1.1",
              "OfflineDeviceCount": 0,
              "ConnectedDeviceCount": 2,
              "ActiveDeviceCount": 2, 
              "SamplingDeviceCount": 100
              }]
  }
  ```