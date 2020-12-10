# Register Devices<a name="edge-getting-started-step4"></a>

**Important**  
Device registration is required to use any part of SageMaker Edge Manager\.

To interact with the cloud you will need to register your device with SageMaker Edge Manager\. In this example register two devices to the fleet you created\.

```
sagemaker_client.register_devices(
    DeviceFleetName=device_fleet_name,
    Devices=[
        {          
            "DeviceName": "sample-device-1",
            "IotThingName": "sample-thing-name-1"
        },
        {
            "DeviceName": "sample-device-2",
            "IotThingName": "sample-thing-name-2"
        }
   ]
)
```