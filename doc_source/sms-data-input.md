# Input Data<a name="sms-data-input"></a>

The input data are the data objects that you send to your workforce to be labeled\. Each object in the input data is described in a manifest file\. Each line in the manifest is an entry containing an object to label and may contain labels from previous jobs\.
+ The input data is stored in an Amazon S3 bucket\. The bucket must be in the same region as you are running Amazon SageMaker Ground Truth\. You must give access to Amazon SageMaker for the data to be read\. In order to read the data, give access to Amazon SageMaker\. For more information about Amazon S3 buckets, see [ Working with Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)\. 
+ The manifest file must be in the same region the data files but does not need to be in the same location as the data files\. It can be in any Amazon S3 bucket accessible to the role that you assigned to Ground Truth when you created the labeling job with either the console or the [CreateLabelingJob](API_CreateLabelingJob.md) operation\.

The manifest is a UTF\-8 encoded file where each line is a complete and valid JSON object\. Each line is delimited by a standard line break, \\n or \\r\\n\. Since each line must be a valid JSON object, you can't have unescaped line break characters\. For more information about the data format, see [JSON Lines](http://jsonlines.org/)\.

Each JSON object in the manifest file must contain a key, either `source-ref` or `source`\. The value of the keys are interpreted as follows:
+ source\-ref—The source of the object is the S3 object specified in the value\. This can be used when the object is a binary object, such as an image, or when you have text in individual files\.
+ source—The source of the object is the value\. This can be used when the object is a text value\.

You use the `source-ref` key for image files for a bounding box or semantic segmentation labeling job\. Each image file must be:
+ 6 Mb or smaller
+ 1920 x 1080 pixels or smaller for semantic segmentation

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

## Filtering and Selecting Data \(Console\)<a name="sms-data-filtering"></a>

You can use the Amazon SageMaker console to select a portion of your dataset for labeling\. The data must be stored in an Amazon S3 bucket\. You have three options:
+ Use the full dataset\.
+ Choose a randomly selected sample of the dataset\.
+ Specify a subset of the dataset using a query\.

### Using the Full Dataset<a name="sms-full-dataset"></a>

When you choose to use the full dataset you must provide a manifest file for your data objects\. You can provide the S3 bucket location of the manifest file or you can have the Amazon SageMaker console create the file for you\. Choose **Create a manifest file** to create the file\. The file is stored in the S3 bucket specified in the **Input data location** field\.

### Choosing a Random Sample<a name="sms-random-dataset"></a>

Use a random sample of your dataset when you want to label a random subset of your data\. The dataset is stored in the S3 bucket specified in the ** Input dataset location ** field\. 

Once you have specified the percentage of data objects that you want to include in the sample, choose **Create subset**\. The Amazon SageMaker console randomly picks the data objects for your labeling job\. Once the objects are selected, choose **Use this subset**\. 

The Amazon SageMaker console create a manifest file for the selected data objects\. The **Input dataset location** field is modified to point to the new manifest file\.

### Specifying a Subset<a name="sms-select-dataset"></a>

You can specify a subset of your data objects using an Amazon S3 `SELECT` query on the object file names\. 

The `SELECT` statement of the SQL query is defined for you\. You provide the `WHERE` clause to specify which data objects should be returned\.

For more information about the Amazon S3 `SELECT` statement, see [ Selecting Content from Objects ](https://docs.aws.amazon.com/AmazonS3/latest/dev/selecting-content-from-objects.html)

Choose **Create subset** to start the selection, and then choose **Use this subset** to use the selected data\. 

The Amazon SageMaker console create a manifest file for the selected data objects\. The **Input dataset location** field is modified to point to the new manifest file\.