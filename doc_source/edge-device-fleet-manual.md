# Download and Set Up the Edge Manager Agent Manually<a name="edge-device-fleet-manual"></a>

Download the Edge Manager agent based on your operating system, architecture, and AWS Region\. The agent is periodically updated, so you have the option to choose your agent based on release dates and versions\. Once you have the agent, create a JSON configuration file\. Specify the device IoT thing name, fleet name, device credentials, and other key\-value pairs\. See [Running the Edge Manager agent](#edge-device-fleet-running-agent) for full a list of keys you must specify in the configuration file\. You can run the agent as an executable binary or link against it as a dynamic shared object \(DSO\)\.

## How the agent works<a name="edge-device-fleet-how-agent-works"></a>

The agent runs on the CPU of your devices\. The agent runs inference on the framework and hardware of the target device you specified during the compilation job\. For example, if you compiled your model for the Jetson Nano, the agent supports the GPU in the provided [Deep Learning Runtime](https://github.com/neo-ai/neo-ai-dlr) \(DLR\)\.

The agent is released in binary format for supported operating systems\. Check that your operating system is supported and meets the minimum OS requirement in the following table:

------
#### [ Linux ]

**Version:** Ubuntu 18\.04

**Supported Binary Formats:** x86\-64 bit \(ELF binary\) and ARMv8 64 bit \(ELF binary\)

------
#### [ Windows ]

**Version:** Windows 10 version 1909

**Supported Binary Formats:** x86\-32 bit \(DLL\) and x86\-64 bit \(DLL\)

------

## Installing the Edge Manager agent<a name="edge-device-fleet-installation"></a>

To use the Edge Manager agent, you first must obtain the release artifacts and a root certificate\. The release artifacts are stored in an Amazon S3 bucket in the `us-west-2` Region\. To download the artifacts, specify your operating system \(`<OS>`\) and the `<VERSION>`\.

Based on your operating system, replace `<OS>` with one of the following:


| Windows 32\-bit | Windows 64\-bit | Linux x86\-64 | Linux ARMv8 | 
| --- | --- | --- | --- | 
| windows\-x86 | windows\-x64 | linux\-x64 | linux\-armv8 | 

The `VERSION` is broken into three components: `<MAJOR_VERSION>.<YYYY-MM-DD>-<SHA-7>`, where:
+ `<MAJOR_VERSION>`: The release version\. The release version is currently set to `1`\.
+ `<YYYY-MM-DD>`: The time stamp of the artifacts release\.
+ `<SHA-7>`: The repository commit ID from which the release is built\.

You must provide the `<MAJOR_VERSION>` and the time stamp in `YYYY-MM-DD` format\. We suggest you use the latest artifact release time stamp\.

Run the following in your command line to get the latest time stamp\. Replace `<OS>` with your operating system:

```
aws s3 ls s3://sagemaker-edge-release-store-us-west-2-<OS>/Releases/ | sort -r
```

For example, if you have a Windows 32\-bit OS, run:

```
aws s3 ls s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/ | sort -r
```

This returns:

```
2020-12-01 23:33:36 0 

                    PRE 1.20201218.81f481f/
                    PRE 1.20201207.02d0e97/
```

The return output in this example shows two release artifacts\. The first release artifact file notes that the release version has a major release version of `1`, a time stamp of `20201218` \(in YYYY\-MM\-DD format\), and a `81f481f` SHA\-7 commit ID\.

**Note**  
The preceding command assumes you have configured the AWS Command Line Interface\. For more information, about how to configure the settings that the AWS CLI uses to interact with AWS, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\.

Based on your operating system, use the following commands to install the artifacts:

------
#### [ Windows 32\-bit ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/<VERSION>/<VERSION>.zip .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/<VERSION>/sha256_hex.shasum .
```

------
#### [ Windows 64\-bit ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x64/Releases/<VERSION>/<VERSION>.zip .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x64/Releases/<VERSION>/sha256_hex.shasum .
```

------
#### [ Linux x86\-64 ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-x64/Releases/<VERSION>/<VERSION>.tgz .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-x64/Releases/<VERSION>/sha256_hex.shasum .
```

------
#### [ Linux ARMv8 ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-armv8/Releases/<VERSION>/<VERSION>.tgz .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-armv8/Releases/<VERSION>/sha256_hex.shasum .
```

------

You also must download a root certificate\. This certificate validates model artifacts signed by AWS before loading them onto your edge devices\.

Replace `<OS>` corresponding to your platform from the list of supported operation systems and replace `<REGION>` with your AWS Region\.

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-<OS>/Certificates/<REGION>/<REGION>.pem .
```

## Running the Edge Manager agent<a name="edge-device-fleet-running-agent"></a>

You can run the SageMaker Edge Manager agent as a standalone process in the form of an Executable and Linkable Format \(ELF\) executable binary or you can link against it as a dynamic shared object \(\.dll\)\. Linux supports running it as a standalone executable binary and is the preferred mode\. Windows supports running it as a shared object \(\.dll\)\.

On Linux, we recommend that you run the binary via a service that’s a part of your initialization \(`init`\) system\. If you want to run the binary directly, you can do so in a terminal as shown in the following example\. If you have a modern OS, there are no other installations necessary prior to running the agent, since all the requirements are statically built into the executable\. This gives you flexibility to run the agent on the terminal, as a service, or within a container\.

To run the agent, first create a JSON configuration file\. Specify the following key\-value pairs:
+ `sagemaker_edge_core_device_name`: The name of the device\. This device name needs to be registered along with the device fleet in the SageMaker Edge Manager console\.
+ `sagemaker_edge_core_device_fleet_name`: The name of the fleet to which the device belongs\.
+ `sagemaker_edge_core_region`: The AWS Region associated with the device, the fleet and the Amazon S3 buckets\. This corresponds to the Region where the device is registered and where the Amazon S3 bucket is created \(they are expected to be the same\)\. The models themselves can be compiled with SageMaker Neo in a different Region, this configuration is not related to model compilation Region\.
+ `sagemaker_edge_core_root_certs_path`: The absolute folder path to root certificates\. This is used to validate the device with the relevant AWS account\.
+ `sagemaker_edge_provider_aws_ca_cert_file`: The absolute path to Amazon Root CA certificate \(AmazonRootCA1\.pem\)\. This is used to validate the device with the relevant AWS account\. `AmazonCA` is a certificate owned by AWS\.
+ `sagemaker_edge_provider_aws_cert_file`: The absolute path to AWS IoT signing root certificate \(`*.pem.crt`\)\.
+ `sagemaker_edge_provider_aws_cert_pk_file`: The absolute path to AWS IoT private key\. \(`*.pem.key`\)\.
+ `sagemaker_edge_provider_aws_iot_cred_endpoint`: The AWS IoT credentials endpoint \(*identifier*\.iot\.*region*\.amazonaws\.com\)\. This endpoint is used for credential validation\. See [Connecting devices to AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/iot-connect-devices.html) for more information\.
+ `sagemaker_edge_provider_provider`: This indicates the implementation of the provider interface being used\. The provider interface communicates with the end network services for uploads, heartbeats and registration validation\. By default this is set to `"Aws"`\. We allow custom implementations of the provider interface\. It can be set to `None` for no provider or `Custom` for custom implementation with the relevant shared object path provided\.
+ `sagemaker_edge_provider_provider_path`: Provides the absolute path to the provider implementation shared object\. \(\.so or \.dll file\)\. The `"Aws"` provider \.dll or \.so file is provided with the agent release\. This field is mandatory\.
+ `sagemaker_edge_provider_s3_bucket_name`: The name of your Amazon S3 bucket \(not the Amazon S3 bucket URI\)\. The bucket must have a `sagemaker` string within its name\.
+ `sagemaker_edge_log_verbose` \(Boolean\.\): Optional\. This sets the debug log\. Select either `True` or `False`\.
+ `sagemaker_edge_telemetry_libsystemd_path`: For Linux only, `systemd` implements the agent crash counter metric\. Set the absolute path of libsystemd to turn on the crash counter metric\. You can find the default libsystemd path can be found by running `whereis libsystemd` in the device terminal\.
+ `sagemaker_edge_core_capture_data_destination`: The destination for uploading capture data\. Choose either `"Cloud"` or `"Disk"`\. The default is set to `"Disk"`\. Setting it to `"Disk"` writes the input and output tensor\(s\) and auxiliary data to the local file system at your preferred location of\. When writing to `"Cloud"` use the Amazon S3 bucket name provided in the `sagemaker_edge_provider_s3_bucket_name` configuration\.
+ `sagemaker_edge_core_capture_data_disk_path`: Set the absolute path in the local file system, into which capture data files are written when `"Disk"` is the destination\. This field is not used when `"Cloud"` is specified as the destination\.
+ `sagemaker_edge_core_folder_prefix`: The parent prefix in Amazon S3 where captured data is stored when you specify `"Cloud"` as the capture data destination \(`sagemaker_edge_core_capture_data_disk_path)`\. Captured data is stored in a sub\-folder under `sagemaker_edge_core_capture_data_disk_path` if `"Disk"` is set as the data destination\.
+ `sagemaker_edge_core_capture_data_buffer_size` \(Integer value\) : The capture data circular buffer size\. It indicates the maximum number of requests stored in the buffer\.
+ `sagemaker_edge_core_capture_data_batch_size` \(Integer value\): The capture data batch size\. It indicates the size of a batch of requests that are handled from the buffer\. This value must to be less than `sagemaker_edge_core_capture_data_buffer_size`\. A maximum of half the size of the buffer is recommended for batch size\.
+ `sagemaker_edge_core_capture_data_push_period_seconds` \(Integer value\): The capture data push period in seconds\. A batch of requests in the buffer is handled when there are batch size requests in the buffer, or when this time period has completed \(whichever comes first\)\. This configuration sets that time period\.
+ `sagemaker_edge_core_capture_data_base64_embed_limit`: The limit for uploading capture data in bytes\. Integer value\.

Your configuration file should look similar to the following example\(with your specific values specified\)\. This example uses the default AWS provider\(`"Aws"`\) and does not specify a periodic upload\.

```
{
    "sagemaker_edge_core_device_name": "device-name",
    "sagemaker_edge_core_device_fleet_name": "fleet-name",
    "sagemaker_edge_core_region": "region",
    "sagemaker_edge_core_root_certs_path": "<Absolute path to root certificates>",
    "sagemaker_edge_provider_provider": "Aws",
    "sagemaker_edge_provider_provider_path" : "/path/to/libprovider_aws.so",
    "sagemaker_edge_provider_aws_ca_cert_file": "<Absolute path to Amazon Root CA certificate>/AmazonRootCA1.pem",
    "sagemaker_edge_provider_aws_cert_file": "<Absolute path to AWS IoT signing root certificate>/device.pem.crt",
    "sagemaker_edge_provider_aws_cert_pk_file": "<Absolute path to AWS IoT private key.>/private.pem.key",
    "sagemaker_edge_provider_aws_iot_cred_endpoint": "https://<AWS IoT Endpoint Address>",
    "sagemaker_edge_core_capture_data_destination": "Cloud",
    "sagemaker_edge_provider_s3_bucket_name": "sagemaker-bucket-name",
    "sagemaker_edge_core_folder_prefix": "Amazon S3 folder prefix",
    "sagemaker_edge_core_capture_data_buffer_size": 30,
    "sagemaker_edge_core_capture_data_batch_size": 10,
    "sagemaker_edge_core_capture_data_push_period_seconds": 4000,
    "sagemaker_edge_core_capture_data_base64_embed_limit": 2,
    "sagemaker_edge_log_verbose": false
}
```

The release artifact includes a binary executable called `sagemaker_edge_agent_binary` in the `/bin` directory\. To run the binary, use the `-a` flag to create a socket file descriptor \(\.sock\) in a directory of your choosing and specify the path of the agent JSON config file you created with the `-c` flag\.

```
./sagemaker_edge_agent_binary -a <ADDRESS_TO_SOCKET> -c <PATH_TO_CONFIG_FILE>
```

The following example shows the code snippet with a directory and file path specified:

```
./sagemaker_edge_agent_binary -a /tmp/sagemaker_edge_agent_example.sock -c sagemaker_edge_config.json
```

In this example, a socket file descriptor named `sagemaker_edge_agent_example.sock` is created in the `/tmp` directory and points to a configuration file that is in the same working directory as the agent called `sagemaker_edge_config.json`\.