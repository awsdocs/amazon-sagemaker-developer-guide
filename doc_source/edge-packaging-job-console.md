# Package a Model \(Amazon SageMaker Console\)<a name="edge-packaging-job-console"></a>

You can create a SageMaker Edge Manager packaging job using the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\. Before continuing, make sure you have satisfied the [Prerequisites](edge-packaging-job.md#edge-packaging-job-prerequisites)\.

1. In the SageMaker console, choose **Edge Inference** and then choose **Edge packaging jobs**, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/pre-edge-packaging-button-edited.png)

1. On the **Job properties** page, enter a name for your packaging job under **Edge packaging job name**\. Note that Edge Manager packaging job names are case\-sensitive\. Name your model and give it a version: enter this under **Model name** and **Model version**, respectively\.

1. Next, select an **IAM role**\. You can chose a role or let AWS create a role for you\. You can optionally specify a **resource key ARN** and **job tags**\.

1. Choose **Next**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/create-edge-packaging-job-filled.png)

1. Specify the name of the compilation job you used when compiling your model with SageMaker Neo in the **Compilation job name** field\. Choose **Next**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/create-edge-packaging-job-model-source-filled.png)

1. On the **Output configuration** page, enter the Amazon S3 bucket URI in which you want to store the output of the packaging job\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/smith/create-device-fleet-output-filled.png)

   The **Status** column on the **Edge packaging** jobs page should read **IN PROGRESS**\. Once the packaging job is complete, the status updates to **COMPLETED**\.

   Selecting a packaging job directs you to that job's settings\. The **Job settings** section displays the job name, ARN, status, creation time, last modified time, duration of the packaging job, and role ARN\.

   The **Input configuration** section displays the location of the model artifacts, the data input configuration, and the machine learning framework of the model\.

   The **Output configuration** section displays the output location of the packaging job, the target device for which the model was compiled, and any tags you created\.

1. Choose the name of your device fleet to be redirected to the device fleet details\. This page displays the name of the device fleet, ARN, description \(if you provided one\), date the fleet was created, last time the fleet was modified, Amazon S3 bucket URI, AWS KMS key ID \(if provided\), AWS IoT alias \(if provided\), and IAM role\. If you added tags, they appear in the **Device fleet tags** section\.