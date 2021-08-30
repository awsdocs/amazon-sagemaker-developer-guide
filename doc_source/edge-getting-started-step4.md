# Download and Set Up Edge Manager<a name="edge-getting-started-step4"></a>

The Edge Manager agent is an inference engine for your edge devices\. Use the agent to make predictions with models loaded onto your edge devices\. The agent also collects model metrics and captures data at specific intervals\.



In this section you will set up your device with the agent\. To do so, first copy a release artifact and signing root certificate from the release bucket locally to your machine\. After you unzip the release artifact, upload it to Amazon S3\. Next, define and save a configuration file for the agent\. A template is provided for you to copy and paste\. Finally, copy the release artifacts, configuration file, and credentials to your device\.

1. **Download the SageMaker Edge Manager agent\.**

   The agent is released in binary format for supported operating systems\. This example runs inference on a Jetson Nano which uses a Linux operating system and has an ARM64 architecture\. For more information about what operating system and architecture supported devices use, see [Supported Devices, Chip Architectures, and Systems](neo-supported-devices-edge-devices.md)\.

   Fetch the latest version of binaries from the SageMaker Edge Manager release bucket from the us\-west\-2 Region\.

   ```
   !aws s3 ls s3://sagemaker-edge-release-store-us-west-2-linux-armv8/Releases/ | sort -r
   ```

   This returns release artifacts sorted by their version\.

   ```
                              PRE 1.20210512.96da6cc/
                              PRE 1.20210305.a4bc999/
                              PRE 1.20201218.81f481f/
                              PRE 1.20201207.02d0e97/
   ```

   The version has the following format: `<MAJOR_VERSION>.<YYYY-MM-DD>.<SHA-7>`\. It consists of three components:
   + `<MAJOR_VERSION>`: The release version\. The release version is currently set to `1`\.
   + `<YYYY-MM-DD>`: The time stamp of the artifact release\.
   + <SHA\-7>: The repository commit ID from which the release is built\.

   Copy the zipped TAR file locally or to your device directly\. The following example shows how to copy the latest release artifact at the time this document was released\.

   ```
   !aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-x64/Releases/1.20201218.81f481f/1.20201218.81f481f.tgz ./
   ```

   Once you have the artifact, unzip the zipped TAR file\. The following unzips the TAR file and stores it in a directory called `agent_demo`:

   ```
   !mkdir agent_demo
   !tar -xvzf 1.20201218.81f481f.tgz -C ./agent_demo
   ```

   Upload the agent release artifacts to your Amazon S3 bucket\. The following code example copies the content within `agent_demo` and uploads it to a directory within your Amazon S3 bucket called `agent_demo`:

   ```
   !aws s3 cp --recursive ./agent_demo s3://{bucket}/agent_demo
   ```

   You also need the signing root certificates from the release bucket:

   ```
   !aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-x64/Certificates/us-west-2/us-west-2.pem ./
   ```

   Upload the signing root certificate to your Amazon S3 bucket:

   ```
   !aws s3 cp us-west-2.pem s3://{bucket}/authorization-files/
   ```

1. **Define a SageMaker Edge Manager agent configuration file\.**

   First, define the agent configuration file as follows:

   ```
   sagemaker_edge_config = {
       "sagemaker_edge_core_device_name": "device_name",
       "sagemaker_edge_core_device_fleet_name": "device_fleet_name",
       "sagemaker_edge_core_capture_data_buffer_size": 30,
       "sagemaker_edge_core_capture_data_push_period_seconds": 4,
       "sagemaker_edge_core_folder_prefix": "demo_capture",
       "sagemaker_edge_core_region": "us-west-2",
       "sagemaker_edge_core_root_certs_path": "/agent_demo/certificates",
       "sagemaker_edge_provider_aws_ca_cert_file": "/agent_demo/iot-credentials/AmazonRootCA1.pem",
       "sagemaker_edge_provider_aws_cert_file": "/agent_demo/iot-credentials/device.pem.crt",
       "sagemaker_edge_provider_aws_cert_pk_file": "/agent_demo/iot-credentials/private.pem.key",
       "sagemaker_edge_provider_aws_iot_cred_endpoint": "endpoint",
       "sagemaker_edge_provider_provider": "Aws",
       "sagemaker_edge_provider_s3_bucket_name": bucket,
       "sagemaker_edge_core_capture_data_destination": "Cloud"
   }
   ```

   Replace the following:
   + `"device_name"` with the name of your device \(this string was stored in an earlier step in a variable named `device_name`\)\.
   + `"device_fleet_name`" with the name of your device fleet \(this string was stored an earlier step in a variable named `device_fleet_name`\)
   + `"endpoint"` with your AWS account\-specific endpoint for the credentials provider \(this string was stored in an earlier step in a variable named `endpoint`\)\.

   Next, save it as a JSON file:

   ```
   edge_config_file = open("sagemaker_edge_config.json", "w")
   json.dump(sagemaker_edge_config, edge_config_file, indent = 6)
   edge_config_file.close()
   ```

   Upload the configuration file to your Amazon S3 bucket:

   ```
   !aws s3 cp sagemaker_edge_config.json s3://{bucket}/
   ```

1. **Copy the release artifacts, configuration file, and credentials to your device\.**

   The following instructions are performed on the edge device itself\.
**Note**  
You must first install Python, the AWS SDK for Python \(Boto3\), and the AWS CLI on your edge device\. 

   Open a terminal on your device\. Create a folder to store the release artifacts, your credentials, and the configuration file\.

   ```
   mkdir agent_demo
   cd agent_demo
   ```

   Copy the contents of the release artifacts that you stored in your Amazon S3 bucket to your device:

   ```
   # Copy release artifacts 
   aws s3 cp s3://<bucket-name>/agent_demo/ ./ --recursive
   ```

   \(The contents of the release artifact was stored in a directory called `agent_demo` in a previous step\)\. Replace `<bucket-name>` and `agent_demo` with the name of your Amazon S3 bucket and the file path to your release artifacts, respectively\.

   Go the `/bin` directory and make the binary files executable:

   ```
   cd bin
   
   chmod +x sagemaker_edge_agent_binary
   chmod +x sagemaker_edge_agent_client_example
   
   cd agent_demo
   ```

   Make a directory to store your AWS IoT credentials and copy your credentials from your Amazon S3 bucket to your edge device \(use the same on you define in the variable `bucket`:

   ```
   mkdir iot-credentials
   cd iot-credentials
   
   aws s3 cp s3://<bucket-name>/authorization-files/AmazonRootCA1.pem ./
   aws s3 cp s3://<bucket-name>/authorization-files/device.pem.crt ./
   aws s3 cp s3://<bucket-name>/authorization-files/private.pem.key ./
   
   cd ../
   ```

   Make a directory to store your model signing root certificates:

   ```
   mkdir certificates
   
   cd certificates
   
   aws s3 cp s3://<bucket-name>/authorization-files/us-west-2.pem ./
   
   cd agent_demo
   ```

   Copy your configuration file to your device:

   ```
   #Download config file from S3
   aws s3 cp s3://<bucket-name>/sagemaker_edge_config.json ./
   
   cd agent_demo
   ```

   Your `agent_demo` directory on your edge device should look similar to the following:

   ```
   ├──agent_demo
   |    ├── bin
   |        ├── sagemaker_edge_agent_binary
   |        └── sagemaker_edge_agent_client_example
   |    ├── sagemaker_edge_config.json
   |    ├── certificates
   |        └──us-west-2.pem
   |    ├── iot-credentials
   |        ├── AmazonRootCA1.pem
   |        ├── device.pem.crt
   |        └── private.pem.key
   |    ├── docs
   |        ├── api
   |        └── examples
   |    ├── ATTRIBUTIONS.txt
   |    ├── LICENSE.txt  
   |    └── RELEASE_NOTES.md
   ```