# Create a Fleet<a name="edge-getting-started-step3"></a>

Device fleets are collections of logically grouped devices that you can use to analyze your data\. In this example, you create a fleet consisting of two devices\.

To create a fleet, use the `create_device_fleet` API\. You need to provide a name for your fleet and an Amazon S3 bucket URI where you want sampled data taken from your devices to be stored\.

```
device_fleet_name="sample-device-fleet"

sagemaker_client.create_device_fleet(
    DeviceFleetName=device_fleet_name,
    OutputConfig={
        'S3OutputLocation': s3_output_location
    }
)
```