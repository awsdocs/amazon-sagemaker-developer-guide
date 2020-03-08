# Input Data<a name="sms-data-input"></a>

The input data are the data objects that you send to your workforce to be labeled\. Each object in the input data is described in a manifest file\. Each line in the manifest is an entry containing an object to label\. An entry can also contain labels from previous jobs\.

Input data and the manifest file must be stored in Amazon Simple Storage Service \(Amazon S3\)\. Each has specific storage and access requirements, as follows: :
+ The S3 bucket that contains the input data must be in the same AWS Region in which you are running Amazon SageMaker Ground Truth\. You must give Amazon SageMaker access to the data stored in the S3 bucket so that it can read it\. For more information about S3 buckets, see [ Working with Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)\. 
+ The manifest file must be in the same AWS Region as the data files, but it doesn't need to be in the same location as the data files\. It can be stored in any S3 bucket that is accessible to the AWS Identity and Access Management \(IAMIAM\) role that you assigned to Ground Truth when you created the labeling job\.

The manifest is a UTF\-8 encoded file where each line is a complete and valid JSON object\. Each line is delimited by a standard line break, \\n or \\r\\n\. Because each line must be a valid JSON object, you can't have unescaped line break characters\. For more information about data format, see [JSON Lines](http://jsonlines.org/)\.

**Limits:** Each JSON object in the manifest file can be no larger than 100 K characters\. No single attribute within an object can be larger than 20,000 characters\. Attribute names can't begin with `$` \(dollar sign\)\.

Each JSON object in the manifest file must contain one of the following keys: `source-ref` or `source`\. The value of the keys are interpreted as follows:
+ source\-ref—The source of the object is the Amazon S3 object specified in the value\. Use this value when the object is a binary object, such as an image, or when you have text in individual files\. You also use the `source-ref` key for image files for a bounding box or semantic segmentation labeling job\.
+ source—The source of the object is the value\. Use this value when the object is a text value\.

The following is an example of a manifest file for files stored in an S3 bucket:

```
{"source-ref": "S3 bucket location 1"}
{"source-ref": "S3 bucket location 2"}
   ...
{"source-ref": "S3 bucket location n"}
```

The following is an example of a manifest file with the input data stored in the manifest:

```
{"source": "Lorem ipsum dolor sit amet"}
{"source": "consectetur adipiscing elit"}
   ...
{"source": "mollit anim id est laborum"}
```

You can include other key\-value pairs in the manifest file\. These pairs are passed to the output file unchanged\. This is useful when you want to pass information between your applications\. For more information, see [Output Data](sms-data-output.md)\.

## Create a Manifest File<a name="sms-console-create-manifest-file"></a>

You can create a manifest file for your labeling jobs in the Ground Truth console using images, text \(\.txt\) files, and comma\-separated value \(\.csv\) files\. Before using the following procedure, ensure that your input images or files are correctly formatted:
+ Image files – Image files must comply with the size and resolution limits listed in the tables found in [Input File Size Quota](#input-file-size-limit)\. 
+ Text files – Text data can be stored in one or more \.txt files\. Each item that you want labeled must be separated by a standard line break\. 
+ CSV files – Text data can be stored in one or more \.csv files\. Each item that you want labeled must be in a separate row\.

**To create a manifest file**

1. Store the images or text files that you want to have labeled in an S3 bucket that is in the same Region as your labeling job\. You must give access to Amazon SageMaker for the data to be read\. For more information about Amazon S3 buckets, see [ Working with Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)\. 

1. Open the Ground Truth console: [https://console.aws.amazon.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\.

1. Choose **Labeling Job**\. 

1. In the **Job overview** section, under **Input dataset location**, choose **Create manifest file**\.

1. In **Input dataset location**, enter the path to the S3 bucket where you data is stored \(for example, s3://*bucket/path\-to\-your\-objects*\)\. 

1. Choose the **Data type**, then choose **Create**\. You see a loading screen while Ground Truth examines the S3 bucket and creates a *\.manifest* file in the S3 location specified above\. If you would like to visually inspect the auto\-generated manifest file, convert this file into a human\-readable format by downloading a copy of the file and changing the file suffix from *\.manifest* to *\.txt*\. 

1. For images or text object, a green box indicates the number of objects detected\. If you're satisfied with the results, choose **Use this manifest**\. If not, modify the image or text files in your S3 bucket and use this procedure to generate a new manifest file\. 

## Input Data Quotas<a name="input-data-limits"></a>

Input datasets used in semantic segmentation labeling jobs have a quota of 20,000 items\. For all other labeling job types, the dataset size quota is 100,000 items\. To request an increase to the quota for labeling jobs other than semantic segmentation jobs, review the procedures in [AWS Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) to request a quota increase\.

Input image data for active and non\-active learning labeling jobs must not exceed size and resolution quotas\. *Active learning* refers to labeling job that use [automated data labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html)\. *Non\-active learning* refers to labeling jobs that don't use automated data labeling\.

### Input File Size Quota<a name="input-file-size-limit"></a>

Input files can't exceed the following size\- quotas for both active and non\-active learning labeling jobs\.


| Labeling Job | Input File Size Quota | 
| --- | --- | 
| Image classification | 12 MB | 
| Object detection \(Bounding box\)  | 12 MB | 
| Semantic segmentation | 6 MB | 
| Image classification label adjustment | 12 MB | 
| Object detection label adjustment | 12 MB | 
| Semantic segmentation label adjustment | 6 MB | 
| Image classification label verification | 12 MB | 
| Object detection label verification | 12 MB | 
| Semantic segmentation label verification | 6 MB | 

### Input Image Resolution Quotas<a name="non-active-learning-input-data-limits"></a>

Image file resolution refers to the number of pixels in an image, and determines the amount of detail an image holds\. Image resolution quotas differ depending on the labeling job type and the Amazon SageMaker built\-in algorithm used\. The following table lists the resolution quotas for images used in active and non\-active learning labeling jobs\.


| Labeling Job | **Resolution Quota \- Non Active Learning** | Resolution Quota \- Active Learning | 
| --- | --- | --- | 
| Image classification | 7680 × 4320 pixels \(8 KB\) | 3840 x 2160 pixels \(4 KB\) | 
| Object detection \(Bounding box\) | 7680 × 4320 pixels \(8 KB\) | 3840 x 2160 pixels \(4 KB\) | 
| Semantic segmentation | 3840 x 2160 pixels \(4 KB\) | 1920 x 1080 pixels \(1080 p\) | 
| Image classification label adjustment | 7680 × 4320 pixels \(8 KB\) | 3840 x 2160 pixels \(4 KB\) | 
| Object detection label adjustment | 7680 × 4320 pixels \(8 KB\) | 3840 x 2160 pixels \(4 KB\) | 
| Semantic segmentation label adjustment | 3840 x 2160 pixels \(4 KB\) | 1920 x 1080 pixels \(1080 p\) | 
| Image classification label verification | 7680 × 4320 pixels \(8 KB\) | Not available | 
| Object detection label verification | 7680 × 4320 pixels \(8 KB\) | Not available | 
| Semantic segmentation label verification | 3840 x 2160 pixels \(4 KB\) | Not available | 

**Note**  
Active learning isn't available for label verification jobs\.

## Filter and Select Data for Labeling<a name="sms-data-filtering"></a>

You can use the Amazon SageMaker console to select a portion of your dataset for labeling\. The data must be stored in an Amazon S3 bucket\. You have three options:
+ Use the full dataset\.
+ Choose a randomly selected sample of the dataset\.
+ Specify a subset of the dataset using a query\.

The following options are available in the **Labeling jobs** section of the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/groundtruth) after selecting **Create labeling job**\. See [Getting started](sms-getting-started.md) to learn how to Create a labeling job in the console\. To configure the dataset that you use for labeling, in the **Job overview** section, select **Additional configuration**\.

### Use the Full Dataset<a name="sms-full-dataset"></a>

When you choose to use **Full dataset**, you must provide a manifest file for your data objects\. You can provide the path of the S3 bucket that contains the manifest file or you can use the Amazon SageMaker console to create the file\. See [Create a Manifest File ](#sms-console-create-manifest-file) to learn how to create a manifest file using the console\. 

### Choose a Random Sample<a name="sms-random-dataset"></a>

When you want to label a random subset of your data, select **Random sample**\. The dataset is stored in the S3 bucket specified in the ** Input dataset location ** field\. 

After you have specified the percentage of data objects that you want to include in the sample, choose **Create subset**\. Amazon SageMaker randomly picks the data objects for your labeling job\. After the objects are selected, choose **Use this subset**\. 

Amazon SageMaker creates a manifest file for the selected data objects\. It also modifies the value in the **Input dataset location** field to point to the new manifest file\.

### Specify a Subset<a name="sms-select-dataset"></a>

You can specify a subset of your data objects using an Amazon Simple Storage Service \(Amazon S3\) `SELECT` query on the object file names\. 

The `SELECT` statement of the SQL query is defined for you\. You provide the `WHERE` clause to specify which data objects should be returned\.

For more information about the Amazon S3 `SELECT` statement, see [ Selecting Content from Objects\. ](https://docs.aws.amazon.com/AmazonS3/latest/dev/selecting-content-from-objects.html)

Choose **Create subset** to start the selection, and then choose **Use this subset** to use the selected data\. 

Amazon SageMaker creates a manifest file for the selected data objects\. It also updates the value in the **Input dataset location** field to point to the new manifest file\.