# Filter and Select Data for Labeling<a name="sms-data-filtering"></a>

You can use the Amazon SageMaker console to select a portion of your dataset for labeling\. The data must be stored in an Amazon S3 bucket\. You have three options:
+ Use the full dataset\.
+ Choose a randomly selected sample of the dataset\.
+ Specify a subset of the dataset using a query\.

The following options are available in the **Labeling jobs** section of the [SageMaker console](https://console.aws.amazon.com/sagemaker/groundtruth) after selecting **Create labeling job**\. To learn how to create a labeling job in the console, see [Getting started](sms-getting-started.md)\. To configure the dataset that you use for labeling, in the **Job overview** section, choose **Additional configuration**\.

## Use the Full Dataset<a name="sms-full-dataset"></a>

When you choose to use the **Full dataset**, you must provide a manifest file for your data objects\. You can provide the path of the Amazon S3 bucket that contains the manifest file or use the SageMaker console to create the file\. To learn how to create a manifest file using the console, see [Automated Data Setup](sms-data-input.md#sms-console-create-manifest-file)\. 

## Choose a Random Sample<a name="sms-random-dataset"></a>

When you want to label a random subset of your data, select **Random sample**\. The dataset is stored in the Amazon S3 bucket specified in the ** Input dataset location ** field\. 

After you have specified the percentage of data objects that you want to include in the sample, choose **Create subset**\. SageMaker randomly picks the data objects for your labeling job\. After the objects are selected, choose **Use this subset**\. 

SageMaker creates a manifest file for the selected data objects\. It also modifies the value in the **Input dataset location** field to point to the new manifest file\.

## Specify a Subset<a name="sms-select-dataset"></a>

You can specify a subset of your data objects using an Amazon S3 `SELECT` query on the object file names\. 

The `SELECT` statement of the SQL query is defined for you\. You provide the `WHERE` clause to specify which data objects should be returned\.

For more information about the Amazon S3 `SELECT` statement, see [ Selecting Content from Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/selecting-content-from-objects.html)\.

Choose **Create subset** to start the selection, and then choose **Use this subset** to use the selected data\. 

SageMaker creates a manifest file for the selected data objects\. It also updates the value in the **Input dataset location** field to point to the new manifest file\.