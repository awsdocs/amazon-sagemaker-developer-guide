# Prerequisites<a name="edge-greengrass-prerequisites"></a>

SageMaker Edge Manager uses AWS IoT Greengrass V2 to simplify the deployment of the Edge Manager agent, your machine learning models, and your inference application to your device\(s\) with the use of components\. To make it easier to maintain your AWS IAM roles, Edge Manager allows you to reuse your existing AWS IoT role alias\. If you do not have one yet, Edge Manager generates a role alias as part of the Edge Manager packaging job\. You no longer need to associate a role alias generated from the SageMaker Edge Manager packaging job with your AWS IoT Role\. 

Before you start, you must:

1. Install the AWS IoT Greengrass Core software\. For detailed information, see [Install the AWS IoT Greengrass Core software](https://docs.aws.amazon.com/greengrass/v2/developerguide/getting-started.html#install-greengrass-v2)\.

1. Set up AWS IoT Greengrass V2\. For more information, see [Install AWS IoT Greengrass Core software with manual resource provisioning](https://docs.aws.amazon.com/greengrass/v2/developerguide/manual-installation.html)\.
**Note**  
Make sure the AWS IoT thing name is all lowercase\.
IAM Role starts with `SageMaker*`

1. Attach the following permission and inline policy to the IAM role created during AWS IoT Greengrass V2 setup\.
   + Navigate to the IAM console [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
   + Search for the role you created by typing in rhe role name in the **Search** field\.
   + Choose your role\.
   + Next, choose **Attach policies**\.
   + Search for **AmazonSageMakerEdgeDeviceFleetPolicy**\.
   + Select **AmazonSageMakerFullAccess** \(This is an optional step that makes it easier for you to reuse this IAM role in model compilation and packaging\)\.
   + Select **Add inline policy**\.

     ```
     {
         "Version":"2012-10-17",
         "Statement":[
           {
             "Sid":"GreengrassComponentAccess",
             "Effect":"Allow",
             "Action":[
                 "greengrass:CreateComponentVersion",
                 "greengrass:DescribeComponent"
             ],
             "Resource":"*"
            }
         ]
     }
     ```
   + Click **Attach policy**\.
   + Select **Trust relationship**\.
   + Click **Edit trust relationship**\.
   + Replace the content with the following\.

     ```
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "Service": "credentials.iot.amazonaws.com"
           },
           "Action": "sts:AssumeRole"
         },
         {
           "Effect": "Allow",
           "Principal": {
             "Service": "sagemaker.amazonaws.com"
           },
           "Action": "sts:AssumeRole"
         }
       ]
     }
     ```

1. Create an Edge Manager device fleet\. For information on how to create a fleet, see [Set Up Devices and Fleets](edge-device-fleet.md)\.

1. Register your device with the same name as your AWS IoT thing name created during the AWS IoT Greengrass V2 setup\.

1. Create at least one custom private AWS IoT Greengrass component\. This component is the application that runs inference on the device\. See [Create a Hello World custom component](edge-greengrass-custom-component.md#edge-greengrass-create-custom-component-how)

**Note**  
The SageMaker Edge Manager and AWS IoT Greengrass integration only works for AWS IoT Greengrass v2\.
Both your AWS IoT thing name and Edge Manager device name must be the same\.
SageMaker Edge Manager does not load local AWS IoT certificates and call the AWS IoT credential provider endpoint directly\. Instead, SageMaker Edge Manager uses the AWS IoT Greengrass v2 TokenExchangeService and it fetches a temporary credential from a TES endpoint\.