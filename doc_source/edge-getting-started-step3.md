# Create and Register Fleets and Authenticate Devices<a name="edge-getting-started-step3"></a>

In this section you will create your AWS IoT thing object, create a device fleet, register your device fleet so it can interact with the cloud, create X\.509 certificates to authenticate your devices to AWS IoT Core, associate the role alias with AWS IoT that was generated when you created your fleet, get your AWS account\-specific endpoint for the credentials provider, get an official Amazon Root CA file, and upload the Amazon CA file to Amazon S3\.

1. **Create AWS IoT things\.**

   SageMaker Edge Manager takes advantage of the AWS IoT Core services to facilitate the connection between the edge devices and endpoints in the AWS cloud\. You can take advantage of existing AWS IoT functionality after you set up your devices to work with Edge Manager\.

   To connect your device to AWS IoT, you need to create AWS IoT *thing objects*, create and register a client certificate with AWS IoT, and create and configure the IAM role for your devices\.

   First, create AWS IoT thing objects with the AWS IoT client \(`iot_client`\) you created earlier with Boto3\. The following example shows how to create two thing objects:

   ```
   iot_thing_name = 'sample-device'
   iot_thing_type = 'getting-started-demo'
   
   iot_client.create_thing_type(
       thingTypeName=iot_thing_type
   )
   
   # Create an AWS IoT thing objects
   iot_client.create_thing(
       thingName=iot_thing_name,
       thingTypeName=iot_thing_type
   )
   ```

1. **Create your device fleet\.**

   Create a device fleet with the SageMaker client object defined in a previous step\. You can also use the SageMaker console to create a device fleet\.

   ```
   import time
   device_fleet_name="demo-device-fleet" + str(time.time()).split('.')[0]
   device_name="sagemaker-edge-demo-device" + str(time.time()).split('.')[0]
   ```

   Specify your IoT role ARN\. This lets AWS IoT grant temporary credentials to devices\.

   ```
   device_model_directory='device_output'
   s3_device_fleet_output = 's3://{}/{}'.format(bucket, device_model_directory)
   
   sagemaker_client.create_device_fleet(
       DeviceFleetName=device_fleet_name,
       RoleArn=iot_role_arn, # IoT Role ARN specified in previous step
       OutputConfig={
           'S3OutputLocation': s3_device_fleet_output
       }
   )
   ```

   An AWS IoT role alias is created when you create a device fleet\. This role alias is associated with AWS IoT using the `iot_client` object in a later step\.

1. **Register your device fleet\.**

   To interact with the cloud, you need to register your device with SageMaker Edge Manager\. In this example, you register a single device with the fleet you created\. To register the device, you need to provide a device name and the AWS IoT thing name as shown in the following example:

   ```
   # Device name should be 36 characters
   device_name = "sagemaker-edge-demo-device" + str(time.time()).split('.')[0]
   
   sagemaker_client.register_devices(
       DeviceFleetName=device_fleet_name,
       Devices=[
           {
               "DeviceName": device_name,
               "IotThingName": iot_thing_name
           }
       ]
   )
   ```

1. **Create X\.509 certificates\.**

   After creating the AWS IoT thing object, you must create a X\.509 device certificate for your thing object\. This certificate authenticates your device to AWS IoT Core\.

   Use the following to create a private key, public key, and a X\.509 certificate file using the AWS IoT client defined \(`iot_client`\) earlier\.

   ```
   # Creates a 2048-bit RSA key pair and issues an X.509 # certificate 
   # using the issued public key.
   create_cert = iot_client.create_keys_and_certificate(
       setAsActive=True 
   )
   
   # Get certificate from dictionary object and save in its own
   with open('./device.pem.crt', 'w') as f:
       for line in create_cert['certificatePem'].split('\n'):
           f.write(line)
           f.write('\n')
   # Get private key from dictionary object and save in its own 
   with open('./private.pem.key', 'w') as f:
       for line in create_cert['keyPair']['PrivateKey'].split('\n'):
           f.write(line)
           f.write('\n')
   # Get a private key from dictionary object and save in its own 
   with open('./public.pem.key', 'w') as f:
       for line in create_cert['keyPair']['PublicKey'].split('\n'):
           f.write(line)
           f.write('\n')
   ```

1. **Associate the role alias with AWS IoT\.**

   When you create a device fleet with SageMaker \(`sagemaker_client.create_device_fleet()`\), a role alias is generated for you\. An AWS IoT role alias provides a mechanism for connected devices to authenticate to AWS IoT using X\.509 certificates, and then obtain short\-lived AWS credentials from an IAM role that is associated with an AWS IoT role alias\. The role alias allows you to change the role of the device without having to update the device\. Use `DescribeDeviceFleet` to get the role alias name and ARN\.

   ```
   # Print Amazon Resource Name (ARN) and alias that has access 
   # to AWS Internet of Things (IoT).
   sagemaker_client.describe_device_fleet(DeviceFleetName=device_fleet_name)
   
   # Store iot role alias string in a variable
   # Grabs role ARN
   full_role_alias_name = sagemaker_client.describe_device_fleet(DeviceFleetName=device_fleet_name)['IotRoleAlias']
   start_index = full_role_alias_name.find('SageMaker') # Find beginning of role name  
   role_alias_name = full_role_alias_name[start_index:]
   ```

   Use the `iot_client` to facilitate associating the role alias generated from creating the device fleet with AWS IoT:

   ```
   role_alias = iot_client.describe_role_alias(
                       roleAlias=role_alias_name)
   ```

   For more information about IAM role alias, see [Role alias allows access to unused services](https://docs.aws.amazon.com/iot/latest/developerguide/audit-chk-role-alias-unused-svcs.html) \.

   You created and registered a certificate with AWS IoT earlier for successful authentication of your device\. Now, you need to create and attach a policy to the certificate to authorize the request for the security token\.

   ```
   alias_policy = {
     "Version": "2012-10-17",
     "Statement": {
       "Effect": "Allow",
       "Action": "iot:AssumeRoleWithCertificate",
       "Resource": role_alias['roleAliasDescription']['roleAliasArn']
     }
   }
   
   policy_name = 'aliaspolicy-'+ str(time.time()).split('.')[0]
   aliaspolicy = iot_client.create_policy(policyName=policy_name,
                                          policyDocument=json.dumps(alias_policy))
   
   # Attach policy
   iot_client.attach_policy(policyName=policy_name,
                               target=create_cert['certificateArn'])
   ```

1. **Get your AWS account\-specific endpoint for the credentials provider\.**

   Edge devices need an endpoint in order to assume credentials\. Obtain your AWS account\-specific endpoint for the credentials provider\.

   ```
   # Get the unique endpoint specific to your AWS account that is making the call.
   iot_endpoint = iot_client.describe_endpoint(
       endpointType='iot:CredentialProvider'
   )
   
   endpoint="https://{}/role-aliases/{}/credentials".format(iot_endpoint['endpointAddress'],role_alias_name)
   ```

1. **Get the official Amazon root CA file and upload it to the Amazon S3 bucket\.**

   Use the following in your Jupyter Notebook or AWS CLI \(if you use your terminal, remove the ‘\!’ magic function\):

   ```
   !wget https://www.amazontrust.com/repository/AmazonRootCA1.pem
   ```

   Use the endpoint to make an HTTPS request to the credentials provider to return a security token\. The following example command uses `curl`, but you can use any HTTP client\.

   ```
   !curl --cert device.pem.crt --key private.pem.key --cacert AmazonRootCA1.pem $endpoint
   ```

   If the certificate is verified, upload the keys and certificate to your Amazon S3 bucket URI:

   ```
   !aws s3 cp private.pem.key s3://{bucket}/authorization-files/
   !aws s3 cp device.pem.crt s3://{bucket}/authorization-files/
   !aws s3 cp AmazonRootCA1.pem s3://{bucket}/authorization-files/
   ```

   Clean your working directory by moving your keys and certificate to a different directory:

   ```
   # Optional - Clean up working directory
   !mkdir authorization-files
   !mv private.pem.key device.pem.crt AmazonRootCA1.pem authorization-files/
   ```