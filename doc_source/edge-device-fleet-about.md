# Inference engine \(Edge Manager agent\)<a name="edge-device-fleet-about"></a>

The Edge Manager agent is an inference engine for your edge devices\. Use the agent to make predictions with models loaded onto your edge devices\. The agent also collects model metrics and captures data at specific intervals\. Sample data is stored in your Amazon S3 bucket\.

The agent is released in binary format for all the supported operating systems\. Check your operating system is supported and meets the minimum OS requirement in the following table:

------
#### [ Linux ]

**Version:**Ubuntu 18\.04

**Supported Binary Formats:** x86\-64 bit \(ELF binary\) and ARMv8 64 bit \(ELF binary\)

------
#### [ Windows ]

**Version:**Windows 10 version 1909

**Supported Binary Formats:**x86\-32 bit \(DLL\) and x86\-64 bit \(DLL\)

------

## Installing Edge Manager agent<a name="edge-device-fleet-installation"></a>

To use Edge Manager agent agent, you will first need to obtain the release artifacts\. The release artifacts are stored in an Amazon S3 bucket in the `us-west-2` region\. To download the artifacts, specify your operating system and the `VERSION`\. The `VERSION` is broken into three components: `<MAJOR_VERSION>.<YYYY-MM-DD>-<SHA-7>`, where:
+ `MAJOR_VERSION`: The release version\. The release version is currently set to `1`\.
+ `<YYYY-MM-DD>`: Time stamp of the artifacts release\.
+ `SHA-7`: repository commit ID the release is built from\.

You need to provide the `MAJOR_VERSION` and the time stamp, `<YYYY-MM-DD>`\. We suggest you use the latest artifact release time stamp\. Use the following to get the latest time stamp\.

```
aws s3 ls s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/ | sort -r
```

Based on your operating system, use the following commands to install the artifacts\.

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

You will also need to download a Root Certificate\. This certificate validate model artifacts were signed by AWS before loading them onto your edge devices\.

Replace `{platform_name}` corresponding to your platform from the list of supported operation systems\.

```
# Windows and Linux
aws s3 cp s3://{bucket_name}/Certificates/<VERSION>/<VERSION>.pem .
```

### Running SageMaker Edge Manager agent<a name="edge-device-fleet-running-agent"></a>

SageMaker Edge Manager agent can be run as a standalone process in the form of an Executable and Linkable Format \(ELF\) executable binary or can be linked against as a Dynamic Shared Object \(\.dll\)\. Running as a standalone executable binary is the preferred mode and is supported on Linux\. Running as a shared object \(\.dll\) is supported on Windows\.

On Linux, we recommend to run the binary via a service that’s a part of your initialization \(init\) system\. If you want to run the binary directly, you can do so in a terminal as shown below\. If you have a modern OS, there are no other installations necessary prior to running the agent since all the requirements are statically built into the executable\. This gives you flexibility to run the agent on the terminal, as a service or within a container\.

```
./sagemaker_edge_agent -a <ADDRESS_TO_SOCKET> -c <PATH_TO_CONFIG_FILE>
```

### Obtain AWS IoT Credentials Certificates<a name="edge-device-fleet-agent-certificates"></a>

SageMaker Edge Manager takes advantage of the AWS IoT Core services to facilitate the connection between the edges devices and endpoints in the AWS Cloud\. By doing so, you can take advantage of existing AWS IoT functionality after you set up your devices to work with Edge Manager\.

To connect your device to AWS IoT you will need to: create AWS IoT thing objects, create and register a client certificate with AWS IoT, and create and configure IAM role for your devices\.

Follow the instructions from the [How to Eliminate the Need for Hardcoded AWS Credentials in Devices by Using the AWS IoT Credentials Provider](http://aws.amazon.com/blogs/blogs/security/how-to-eliminate-the-need-for-hardcoded-aws-credentials-in-devices-by-using-the-aws-iot-credentials-provider/) blog post for detailed instructions on create and register a client certificate with AWS IoT, and create and configure IAM role for your devices\. For general information on authorizing AWS services, see [Authorizing direct calls to AWS services](https://docs.aws.amazon.com/iot/latest/developerguide/authorizing-direct-aws.html)\. 

### Models on Edge Devices<a name="edge-device-fleet-agent-models"></a>

The Edge Manager agent can load one model at a time and make inference with that model on edge devices\. The agent validates the model signature and loads into memory all the artifacts produced by the edge packaging job\. This step requires all the required certificates described in previous steps, be installed along with rest of the binary installation\. If the model’s signature cannot be validated then loading of the model fails with appropriate return code and reason\.

See the [Installation](https://docs.aws.amazon.com/sagemaker/latest/dg/edge-device-fleet-installation.html) section for instructions on how to download the root certificate and binary release artifacts\. See, [Obtain AWS IoT Credentials Certificates](https://docs.aws.amazon.com/sagemaker/latest/dg/edge-device-fleet-agent-certificates.html) for information on creating and registering a client certificate with AWS IoT and create and configure IAM role for your devices\.