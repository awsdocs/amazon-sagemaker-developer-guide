# Prerequisites<a name="edge-getting-started-step1"></a>

1. **Set up an AWS Account\.**

   Create an AWS account and create an IAM administrator user\. For instructions on how to set up your AWS account, see [How do I create and activate a new AWS account?](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) For instructions on how to create an administrator user in your AWS account, see [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\.

1. **Set up an IAM Role and attach policies\.**

   SageMaker Edge Manager needs access to your Amazon S3 bucket URI\. To facilitate this, create an IAM role that can run SageMaker and has permission to access your Amazon S3 URI\. You can create an IAM role by using the AWS SDK for Python \(Boto3\), [the SageMaker console](https://console.aws.amazon.com/sagemaker/), or the AWS CLI\. Below is an example of how to create an IAM role using Boto3:

   ```
   import boto3
   import json
   
   AWS_REGION = 'us-west-2'
   
   # Create an IAM client to interact with IAM
   iam_client = boto3.client('iam', region_name=AWS_REGION)
   role_name = 'edge-manager-demo'
   ```

   Create a dictionary describing the IAM policy you are attaching to your IAM role\. The first statement ID in the statement array, `DeviceS3Access`, grants devices access to Amazon S3\. `SageMakerEdgeApis` grants access to APIs you need to get a device heartbeat and to register your device\. You need to use a role alias in a later step in order to authenticate connected devices to AWS IoT using X\.509 certificates\. To do this, include the `CreateIoTRoleAliasIamPermissions` and `CreateIoTRoleAlias` statements\.

   ```
   policy = {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "DeviceS3Access",
               "Effect": "Allow",
               "Action": [
                   "s3:PutObject"
               ],
               "Resource": [
                   "arn:aws:s3:::*SageMaker*",
                   "arn:aws:s3:::*Sagemaker*",
                   "arn:aws:s3:::*sagemaker*"
               ]
           },
           {
               "Sid": "SageMakerEdgeApis",
               "Effect": "Allow",
               "Action": [
                   "sagemaker:SendHeartbeat",
                   "sagemaker:GetDeviceRegistration"
               ],
               "Resource": "*"
           },
           {
               "Sid": "CreateIoTRoleAlias",
               "Effect": "Allow",
               "Action": [
                   "iot:CreateRoleAlias",
                   "iot:DescribeRoleAlias",
                   "iot:UpdateRoleAlias",
                   "iot:ListTagsForResource",
                   "iot:TagResource"
               ],
               "Resource": [
                   "arn:aws:iot:*:*:rolealias/SageMakerEdge*"
               ]
           },
           {
               "Sid": "CreateIoTRoleAliasIamPermissions",
               "Effect": "Allow",
               "Action": [
                   "iam:PassRole",
                   "iam:GetRole"
               ],
               "Resource": [
                   "arn:aws:iam::*:role/*SageMaker*",
                   "arn:aws:iam::*:role/*Sagemaker*",
                   "arn:aws:iam::*:role/*sagemaker*"
               ]
           }
       ]
   }
   ```

   Create a new IAM role using the policy you defined:

   ```
   new_role = iam_client.create_role(
       AssumeRolePolicyDocument=json.dumps(policy),
       Path='/',
       RoleName=role_name,
   )
   ```

   You need your Amazon Resource Name \(ARN\) when you create a packaging job in a later step, so store it in a variable as well\.

   ```
   role_arn = new_role['Role']['Arn']
   ```

   After you create a new role, attach the additional permissions it needs to interact with SageMaker and Amazon S3:

   ```
   iam_client.attach_role_policy(
       RoleName=role_name,
       PolicyArn="arn:aws:iam::aws:policy/AmazonSageMakerFullAccess"
   )
   
   iam_client.attach_role_policy(
       RoleName=role_name,
       PolicyArn="arn:aws:iam::aws:policy/AmazonS3FullAccess"
   );
   ```

1. **Create an Amazon S3 bucket\.**

   Edge Manager accesses your model in Amazon S3 and stores your sample data from your device fleet into Amazon S3\. Create an Amazon S3 bucket with the following:

   ```
   # Create an S3 client so SageMaker can interact with S3
   s3_client = boto3.client("s3", region_name=AWS_REGION)
   
   # Name buckets
   bucket="demo-bucket"
   
   # Create buckets
   s3_client.create_bucket(
       Bucket=bucket,
       CreateBucketConfiguration={
           'LocationConstraint': AWS_REGION
       }
   )
   ```

1. **Train a machine learning model\.**

   See [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html) for more information on how to train a machine learning model using SageMaker\. You can optionally upload your locally trained model directly into an Amazon S3 URI bucket\.

   If you do not have a model yet, use the pre\-trained Keras MobileNetV2 model\.

   ```
   import tensorflow as tf
   
   model = tf.keras.applications.MobileNetV2()
   model.save("mobilenet_v2.h5")
   ```

   The model comes packaged as a Hierarchical Data Format \(HDF\)\. SageMaker expects saved models to be in a compressed tarfile \(\.tar\.gz\) file format\. Repackage the HD5 model into a compressed tarfile with the following:

   ```
   import tarfile
   
   with tarfile.open("mobilenet_v2.tar.gz", mode="w:gz") as archive:
       archive.add("mobilenet_v2.h5")
   ```

1. **Upload your trained model to Amazon S3 bucket\.**

   Once you have a machine learning model, store it in an Amazon S3 bucket\.

   ```
   model_filename= mobilenet_v2.tar.gz
   
   # Upload model        
   s3_client.upload_file(filename=model_filename, bucket=bucket, key=model_filename)
   ```

1. **Compile your model with SageMaker Neo\.**

   Compile your machine learning model with SageMaker Neo for an edge device\. You need to know your Amazon S3 bucket URI where you stored the trained model, the machine learning framework you used to train your model, the shape of your model’s input, and your target device\.

   For the Keras MobileNet V2 model use the following:

   ```
   data_shape = '{"input_1":[1,3,224,224]}'
   framework = 'keras'
   target_device = 'ml_c5'
   ```

   For more information on how to compile your model with SageMaker Neo, see [Compile and Deploy Models with Neo](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html)\.

1. **Create and register AWS IoT thing objects and configure IAM roles\.**

   SageMaker Edge Manager takes advantage of the AWS IoT Core services to facilitate the connection between the edge devices and endpoints in the AWS cloud\. You can take advantage of existing AWS IoT functionality after you set up your devices to work with Edge Manager\.

   To connect your device to AWS IoT you need to create AWS IoT *thing objects*, create and register a client certificate with AWS IoT, and create and configure the IAM role for your devices\.
   + Create AWS IoT thing objects with the following:

     ```
     # Create an IoT client so you can interact with IoT
     iot = boto3.client('iot', region_name=region)
     
     iot.create_thing_type(
         thingTypeName=iot_thing_type
     )
     
     # Create two AWS IoT thing objects
     iot.create_thing(
         thingName='sample-device-1',
         thingTypeName="edge-manager-demo"
     )
     
     iot.create_thing(
         iot_thing_num2_name='sample-device-2',
         thingTypeName="edge-manager-demo"
     )
     ```
   + After creating the AWS IoT thing objects, you will need to create X\.509 device certificate for your thing objects\. This certificate authenticates your device to AWS IoT Core\.

     Use the following to create a private key, public key, and X\.509 certificate file\.

     ```
     # Creates a 2048-bit RSA key pair and issues an X.509 
     # certificate using the issued public key.
     create_cert = iot.create_keys_and_certificate(
         setAsActive=True 
     )
     
     # Get certificate from dictionary object and save in its own file
     with open('./device.pem.crt', 'w') as f:
         for line in create_cert['certificatePem'].split('\n'):
             f.write(line)
             f.write('\n')
     
     # Get private key from dictionary object and save in its own file
     with open('./private.pem.key', 'w') as f:
         for line in create_cert['keyPair']['PrivateKey'].split('\n'):
             f.write(line)
             f.write('\n')
     
     # Get a private key from dictioanary object and save in its own file
     with open('./public.pem.key', 'w') as f:
         for line in create_cert['keyPair']['PublicKey'].split('\n'):
             f.write(line)
             f.write('\n')
     ```
   + Create an AWS IoT policy document\. This policy authorizes your device to interact with AWS IoT services\.

     To do this, create a role alias so that you can change the role of the device without having to update the device\. An AWS IoT role alias provides a mechanism for connected devices to authenticate to AWS IoT using X\.509 certificates and then obtain short\-lived AWS credentials from an IAM role that is associated with an AWS IoT role alias\.

     ```
     device_role_name="demo-role-name"
     
     role_alias_name = device_role_name + "-alias"
     
     # Create role alias with boto3 iot client
     role_alias = iot.create_role_alias(
         roleAlias=role_alias_name,
         roleArn=role_arn, 
         credentialDurationSeconds=3600
     )
     
     # Define policy permissions
     alias_policy = {
       "Version": "2012-10-17",
       "Statement": {
         "Effect": "Allow",
         "Action": "iot:AssumeRoleWithCertificate",
         "Resource": role_alias['roleAliasArn']
       }
     }
     
     # Create the policy witht the permissions defined
     aliaspolicy = iot.create_policy(
         policyName='aliaspolicy',
         policyDocument=json.dumps(alias_policy),
     )
     
     # Attach the policy to client
     iot.attach_policy(
         policyName='aliaspolicy',
         target=create_cert['certificateArn']
     )
     ```

     Get your AWS account\-specific endpoint for the credentials provider\. Edge devices need the endpoint in order to assume credentials\.

     ```
     # Get the unique endpoint specific to your AWS account that is making the call.
     iot_endpoint = iot.describe_endpoint(
         endpointType='iot:CredentialProvider'
     )
     
     endpoint = "https://{}/role-aliases/{}/credentials".format(iot_endpoint['endpointAddress'],role_alias_name)
     ```

     Use the endpoint to make an HTTPS request to the credentials provider to return a security token\. The following example command uses `curl,` but you can use any HTTP client\.

     ```
     !curl --cert iot.pem.crt --key iot_key.pem.key --cacert AmazonRootCA1.pem $endpoint
     ```

     If the certificate is verified, upload the keys and certificate to your Amazon S3 bucket URI\.

     ```
     # Organize S3 bucket, define a folder to store IoT keys and certificate
     iot_certificates = folder + "/iot_certificates"
     s3_client.upload_file(filename="public.pem.key", bucket=bucket,key=iot_certificates)
     s3_client.upload_file(filename="device.pem.key", bucket=bucket,key=iot_certificates)
     s3_client.upload_file(filename="AmazonRootCA1.pem", bucket=bucket,key=iot_certificates)
     ```