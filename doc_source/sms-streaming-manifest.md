# Create a Manifest File \(Optional\)<a name="sms-streaming-manifest"></a>

When you create a streaming labeling job, you have the one time option to add objects \(such as images or text\) to an input manifest file that you specify in `ManifestS3Uri` of `CreateLabelingJob`\. When the streaming labeling job starts, these objects are sent to workers or added to the Amazon SQS queue if the total number of objects exceed `MaxConcurrentTaskCount`\. The results are added to the Amazon S3 path that you specify when creating the labeling job periodically as workers complete labeling tasks\. Output data is sent to any endpoint that you subscribe to your output topic\. 

If you want to provide initial objects to be labeled, create a manifest file that identifies these objects and place it in Amazon S3\. Specify the S3 URI of this manifest file in `ManifestS3Uri` within `InputConfig`\.

To learn how to format your manifest file, see [Input Data](sms-data-input.md)\. To use the SageMaker console to automatically generate a manifest file \(not supported for 3D point cloud task types\), see [Automated Data Setup](sms-console-create-manifest-file.md)\.