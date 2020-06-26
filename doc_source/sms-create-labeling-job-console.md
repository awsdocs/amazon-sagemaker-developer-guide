# Create a Labeling Job \(Console\)<a name="sms-create-labeling-job-console"></a>

You can use the Amazon SageMaker console to create a labeling job for all of the Ground Truth built\-in task types and custom labeling workflows\. 

You need to provide the following to create a labeling job in the Amazon SageMaker console: 
+ An input manifest file in Amazon S3\. For text and image labeling jobs, Ground Truth can generate a manifest file using your input data in Amazon S3\. To learn more, see [Create a Manifest File ](sms-data-input.md#sms-console-create-manifest-file)\. 

  For point cloud labeling job modalities, you need to create a point cloud frame or sequence input manifest file\. To learn more, see [3D Point Cloud Input Data](sms-point-cloud-input-data.md)\.
+ An Amazon S3 bucket to store your output data\. 
+ An IAM role with permission to access your resources in Amazon S3 and an Amazon SageMaker execution policy attached\. For a general solution, you can attach the managed policy, AmazonSageMakerFullAccess, to an IAM role\. For more details see [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)\.
+ If you are using your own private workers, you must create a work team and provide your work team ARN when creating the labeling job\. 
+ If you are using a custom labeling workflow, you must save a worker task template in Amazon S3 and provide an Amazon S3 URI for that template\. For more information, see [Step 2: Creating your custom labeling task template](sms-custom-templates-step2.md)\.
+ \(Optional\) An AWS KMS key ARN if you want Amazon SageMaker to encrypt the output of your labeling job using your own AWS KMS encryption key instead of the default Amazon S3 service key\.
+ \(Optional\) Existing labels for the dataset you use for your labeling job\. Use this option if you want workers to adjust, or approve and reject labels\. 

**Important**  
Your work team, input manifest file, output bucket, and other resources in Amazon S3 must be in the same AWS Region you use to create your labeling job\. 

When you create a labeling job using the Amazon SageMaker console, you can add worker instructions to one of the default template provided by Ground Truth\. To preview the worker UI that is generated from these templates, see the page for your [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html)\. 

**To create a labeling job \(console\)**

1. Sign in to the Amazon SageMaker console at [https://console.aws.amazon.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\. 

1. In the left navigation pane, choose **Labeling jobs**\. 

1. On the **Labeling jobs** page, choose **Create labeling job**\.

1. For **Job name**, enter a name for your labeling job\.

1. \(Optional\) If you want to identify your labels with a key, select **I want to specify a label attribute name different from the labeling job name**\. If you do not select this option, the labeling job name you specified in the previous step will be used to identify your labels in your output manifest file\. 

1. For **Input dataset location**, provide the location in Amazon S3 where your input manifest file is located\. For example, if your input manifest file, manifest\.json, is located in example\-bucket, enter **s3://example\-bucket/manifest\.json**\. 

1. For **Output dataset location**, provide the location in Amazon S3 where you want Ground Truth to store the output data from your labeling job\. 

1. For **IAM Role**, choose an existing IAM role or create an IAM role with permission to access your resources in Amazon S3, to write to the output Amazon S3 bucket specified above, and with an Amazon SageMaker execution policy attached\. 

1. \(Optional\) For **Additional configuration**, you can specify how much of your dataset you want workers to label, and if you want Amazon SageMaker to encrypt the output data for your labeling job using an AWS KMS encryption key\. To encrypt your output data, you must have the required AWS KMS permissions attached to the IAM role you provided in the previous step\. For more details, see [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)\. 

1. In the **Task type** section, under **Task category** use the drop\-down menu to select your task category\. 

1. In **Task selection**, choose your task type\. 

1. \(Optional\) Provide tags for your labeling job to make it easier to find in the console later\. 

1. Choose **Next**\. 

1. In the **Workers** section, choose the type of workforce you would like to use\. For more details about your workforce options see [Create and Manage Workforces](sms-workforce-management.md)\.

1. \(Optional\) After you've selected your workforce, specify the **Task timeout**\. This is the maximum amount of time a worker has to work on a task\.

   For 3D point cloud annotation tasks, the default task timeout is 3 days\. The default timeouts for text and image classification, and label verification labeling jobs is 5 minutes\. The default timeouts for all other labeling jobs is 60 minutes\.

   If you set your task timeout to be greater than one hour, you must [increases the max session duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration) of your execution role to be greater than or equal to the task timeout\. If you choose a task time that is greater than 8 hours for 3D point cloud labeling jobs, refer to [Increase MaxSessionDuration for Execution Role](sms-point-cloud-general-information.md#sms-3d-pointcloud-maxsessduration)\.

1. \(Optional\) For bounding box, semantic segmentation, and point cloud task types, you can select **Display existing labels** if you want to display labels for your input data set for workers to verify or adjust\. If you choose this option, select the label attribute name for the labels that you want to verify or adjust\. This can be found in the output manifest file in your Amazon S3 bucket\. For more information, see [Verify and Adjust Labels ](sms-verification-data.md)\.

1. In the next section, specify your worker instructions and labels\. For the point cloud labeling modality, you can also specify label attributes\. You can select **See preview** to preview your worker instructions, labels, and interact with the worker UI\. For more details about the worker UI for each task type, see the page for your [built\-in task type](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html)\. 

1. \(Optional\) If you are *not* using a point cloud task type, you can add **Additional instructions** to help your worker complete your task\.

1. Choose **Create**\.

After you've successfully created your labeling job, you are redirected to the **Labeling jobs** page\. The status of the labeling job you just created will be **In progress**\. This status progressively updates as workers complete your tasks\. When all tasks are successfully completed, the status changes to **Completed**\. 

If an issue occurred while creating the labeling job, its status changes to **Failed**\.

To view more details about the job, choose the labeling job name\. 

## Next Steps<a name="sms-create-labeling-job-console-next-steps"></a>

After your labeling job status changes to **Completed**, you can view your output data in the Amazon S3 bucket that you specified while creating that labeling job\. For details about the format of your output data, see [Output Data](sms-data-output.md)\.