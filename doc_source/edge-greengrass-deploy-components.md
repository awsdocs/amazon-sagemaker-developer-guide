# Deploy the components to your device<a name="edge-greengrass-deploy-components"></a>

Deploy your components with the AWS IoT console or with the AWS CLI\.

## To deploy your components \(console\)<a name="collapsible-section-gg-deploy-console"></a>

Deploy your AWS IoT Greengrass components with the AWS IoT console\.

1. In the AWS IoT Greengrass console at [https://console\.aws\.amazon\.com/iot/](https://console.aws.amazon.com/greengrass/) navigation menu, choose **Deployments**\.

1. On the **Components** page, on the **Public components** tab, choose `aws.greengrass.SageMakerEdgeManager`\.

1. On the `aws.greengrass.SageMakerEdgeManager` page, choose **Deploy**\.

1. From `Add to deployment`, choose one of the following:

   1. To merge this component to an existing deployment on your target device, choose **Add to existing deployment**, and then select the deployment that you want to revise\.

   1. To create a new deployment on your target device, choose **Create new deployment**\. If you have an existing deployment on your device, choosing this step replaces the existing deployment\.

1. On the **Specify target** page, do the following:

   1. Under **Deployment information**, enter or modify the friendly name for your deployment\.

   1. Under **Deployment targets**, select a target for your deployment, and choose **Next**\. You cannot change the deployment target if you are revising an existing deployment\.

1. On the **Select components** page, under **My components**, choose:
   + com\.*<CUSTOM\-COMPONENT\-NAME>*
   + `aws.greengrass.SageMakerEdgeManager`
   + SagemakerEdgeManager\.*<YOUR\-PACKAGING\-JOB>*

1. On the **Configure components** page, choose **com\.greengrass\.SageMakerEdgeManager**, and do the following\.

   1. Choose **Configure component**\.

   1. Under **Configuration update**, in **Configuration to merge**, enter the following configuration\.

      ```
      {
          "DeviceFleetName": "device-fleet-name",
          "BucketName": "DOC-EXAMPLE-BUCKET"
      }
      ```

      Replace *`device-fleet-name`* with the name of the edge device fleet that you created, and replace *`DOC-EXAMPLE-BUCKET`* with the name of the Amazon S3 bucket that is associated with your device fleet\.

   1. Choose **Confirm**, and then choose **Next**\.

1. On the **Configure advanced settings** page, keep the default configuration settings, and choose **Next**\.

1. On the **Review** page, choose **Deploy**\.

## To deploy your components \(AWS CLI\)<a name="collapsible-section-gg-deploy-cli"></a>

1. Create a ` deployment.json` file to define the deployment configuration for your SageMaker Edge Manager components\. This file should look like the following example\.

   ```
   {
     "targetArn":"targetArn",
     "components": {
       "aws.greengrass.SageMakerEdgeManager": {
         "componentVersion": 1.0.0,
         "configurationUpdate": {
           "merge": {
             "DeviceFleetName": "device-fleet-name",
             "BucketName": "DOC-EXAMPLE-BUCKET"
           }
         }
       },
       "com.greengrass.SageMakerEdgeManager.ImageClassification": {
         "componentVersion": 1.0.0,
         "configurationUpdate": {
         }
       }, 
       "com.greengrass.SageMakerEdgeManager.ImageClassification.Model": {
         "componentVersion": 1.0.0,
         "configurationUpdate": {
         }
       }, 
     }
   }
   ```
   + In the `targetArn` field, replace *`targetArn`* with the Amazon Resource Name \(ARN\) of the thing or thing group to target for the deployment, in the following format:
     + Thing: `arn:aws:iot:region:account-id:thing/thingName`
     + Thing group: `arn:aws:iot:region:account-id:thinggroup/thingGroupName`
   + In the `merge` field, replace *`device-fleet-name`* with the name of the edge device fleet that you created, and replace *`DOC-EXAMPLE-BUCKET`* with the name of the Amazon S3 bucket that is associated with your device fleet\.
   + Replace the component versions for each component with the latest available version\.

1. Run the following command to deploy the components on the device:

   ```
   aws greengrassv2 create-deployment \
       --cli-input-json file://path/to/deployment.json
   ```

The deployment can take several minutes to complete\. In the next step, check the component log to verify that the deployment completed successfully and to view the inference results\.

For more information about deploying components to individual devices or groups of devices, see [Deploy AWS IoT Greengrass components to devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/manage-deployments.html)\.