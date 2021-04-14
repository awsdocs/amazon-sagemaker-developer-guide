# Run Agent<a name="edge-getting-started-step5"></a>

In this section you will run the agent as a binary using gRPC, and check that both your device and fleet are working and collecting sample data\.

1. **Launch the agent\.**

   The SageMaker Edge Manager agent can be run as a standalone process in the form of an Executable and Linkable Format \(ELF\) executable binary or can be linked against as a Dynamic Shared Object \(\.dll\)\. Running as a standalone executable binary is the preferred mode and is supported on Linux\.

   This example uses gRPC to run the agent\. gRPC is an open source high\-performance Remote Procedure Call \(RPC\) framework that can run in any environment\. For more information about gRPC, see the [gRPC documentation](https://grpc.io/docs/)\.

   To use gRPC, perform the following steps: 

   1. Define a service in a \.proto file\.

   1. Generate server and client code using the protocol buffer compiler\.

   1. Use the Python \(or other languages supported by gRPC\) gRPC API to write the server for your service\.

   1. Use the Python \(or other languages supported by gRPC\) gRPC API to write a client for your service\. 

   The release artifact you downloaded contains a gRPC application ready for you to run the agent\. The example is located within the `/bin` directory of your release artifact\. The `sagemaker_edge_agent_binary` binary executable is in this directory\.

   To run the agent with this example, provide the path to your socket file \(\.sock\) and JSON \.config file:

   ```
   ./bin/sagemaker_edge_agent_binary -a /tmp/sagemaker_edge_agent_example.sock -c sagemaker_edge_config.json
   ```

1. **Check your device\.**

   Check that your device is connected and sampling data\. Making periodic checks, manually or automatically, allows you to check that your device or fleet is working properly\.

   Provide the name of the fleet to which the device belongs and the unique device identifier\. From your local machine, run the following:

   ```
   sagemaker_client.describe_device(
       DeviceName=device_name,
       DeviceFleetName=device_fleet_name
   )
   ```

   For the given model, you can see the name, model version, latest sample time, and when the last inference was made\.

   ```
   { 
     "DeviceName": "sample-device",
     "DeviceFleetName": "demo-device-fleet",
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

1. **Check your fleet\.**

   Check that your fleet is working with `GetDeviceFleetReport`\. Provide the name of the fleet the device belongs to\.

   ```
   sagemaer_client.get_device_fleet_report(
       DeviceFleetName=device_fleet_name
   )
   ```

   For a given model, you can see the name, model version, latest sample time, and when the last inference was made, along with the Amazon S3 bucket URI where the data samples are stored\.

   ```
   # Sample output
   {
    "DeviceFleetName": "sample-device-fleet",
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