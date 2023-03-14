# Reusing Data Flows for Different Datasets<a name="data-wrangler-parameterize"></a>

For Amazon Simple Storage Service \(Amazon S3\) data sources, you can create and use parameters\. A parameter is a variable that you've saved in your Data Wrangler flow\. Its value can be any portion of the data source's Amazon S3 path\. Use parameters to quickly change the data that you're importing into a Data Wrangler flow or exporting to a processing job\.

After you created a Data Wrangler flow, you might have trained a model on the data that you've transformed\. For datasets that have the same schema, you can use parameters to apply the same transformations on a different dataset and train a different model\. You can use the new datasets to perform inference with your model or you could be using them to retrain your model\.

In general, parameters have the following attributes:
+ Name – The name you specify for the parameter
+ Type – The type of value that the parameter represents
+ Default value – The value of the parameter when you don't specify a new value

**Note**  
Datetime parameters have a time range attribute that they use as the default value\.

Data Wrangler uses curly braces, `{{}}`, to indicate that a parameter is being used in the Amazon S3 path\. For example, you can have a URL such as `s3://DOC-EXAMPLE-BUCKET1/{{example_parameter_name}}/example-dataset.csv`\.

You create a parameter when you're editing the Amazon S3 data source that you've imported\. You can set any portion of the file path to a parameter value\. You can set the parameter value to either a value or a pattern\. The following are the available parameter value types in the Data Wrangler flow:
+ Number
+ String
+ Pattern
+ Datetime

**Note**  
You can't create a pattern parameter or a datetime parameter for the name of the bucket in the Amazon S3 path\.

You must set a number as the default value of a number parameter\. You can change the value of the parameter to a different number when you're editing a parameter or when you're launching a processing job\. For example, in the S3 path, `s3://DOC-EXAMPLE-BUCKET/example-prefix/example-file-1.csv`, you can create a number parameter named `number_parameter` in the place of `1`\. Your S3 path now appears as `s3://DOC-EXAMPLE-BUCKET/example-prefix/example-file-{{number_parameter}}.csv`\. The path continues to point to the `example-file-1.csv` dataset until you change the value of the parameter\. If you change the value of `number_parameter` to `2` the path is now `s3://DOC-EXAMPLE-BUCKET/example-prefix/example-file-2.csv`\. You can import `example-file-2.csv` into Data Wrangler if you've uploaded the file to that Amazon S3 location\.

A string parameter stores a string as its default value\. For example, in the S3 path, `s3://DOC-EXAMPLE-BUCKET/example-prefix/example-file-1.csv`, you can create a string parameter named `string_parameter` in the place of the filename, `example-file-1.csv`\. The path now appears as `s3://DOC-EXAMPLE-BUCKET/example-prefix/{{string_parameter}}`\. It continues to match `s3://DOC-EXAMPLE-BUCKET/example-prefix/example-file-1.csv`, until you change the value of the parameter\.

Instead of specifying the filename as a string parameter, you can create a string parameter using the entire Amazon S3 path\. You can specify a dataset from any Amazon S3 location in the string parameter\.

A pattern parameter stores a regular expression \(Python REGEX\) string as its default value\. You can use a pattern parameter to import multiple data files at the same time\. To import more than one object at a time, specify a parameter value that matches the Amazon S3 objects that you're importing\.

You can also create a pattern parameter for the following datasets:
+ s3://*DOC\-EXAMPLE\-BUCKET1*/example\-prefix/example\-file\-1\.csv
+ s3://*DOC\-EXAMPLE\-BUCKET1*/example\-prefix/example\-file\-2\.csv
+ s3://*DOC\-EXAMPLE\-BUCKET1*/example\-prefix/example\-file\-10\.csv
+ s3://*DOC\-EXAMPLE\-BUCKET*/example\-prefix/example\-file\-0123\.csv

For `s3://DOC-EXAMPLE-BUCKET1/example-prefix/example-file-1.csv`, you can create a pattern parameter in the place of `1`, and set the default value of the parameter to `\d+`\. The `\d+` REGEX string matches any one or more decimal digits\. If you create a pattern parameter named `pattern_parameter`, your S3 path appears as `s3://DOC-EXAMPLE-BUCKET1/example-prefix/example-file-{{pattern_parameter}}.csv`\.

You can also use pattern parameters to match all CSV objects within your bucket\. To match all objects in a bucket, create a pattern parameter with the default value of `.*` and set the path to `s3://DOC-EXAMPLE-BUCKET/{{pattern_parameter}}.csv`\. The `.*` character matches any string character in the path\. 

The `s3://DOC-EXAMPLE-BUCKET/{{pattern_parameter}}.csv` path can match the following datasets\.
+ `example-file-1.csv`
+ `other-example-file.csv`
+ `example-file-a.csv`

A datetime parameter stores the format with the following information:
+ A format for parsing strings inside an Amazon S3 path\.
+ A relative time range to limit the datetime values that match

For example, in the Amazon S3 file path, `s3://DOC-EXAMPLE-BUCKET/2020/01/01/example-dataset.csv`, 2020/01/01 represents a datetime in the format of `year/month/day`\. You can set the parameter’s time range to an interval such as `1 years` or `24 hours`\. An interval of `1 years` matches all S3 paths with datetimes that fall between the current time and the time exactly a year before the current time\. The current time is the time when you start exporting the transformations that you've made to the data\. For more information about exporting data, see [Export](data-wrangler-data-export.md)\. If the current date is 2022/01/01 and the time range is `1 years`, the S3 path matches datasets such as the following:
+ s3://*DOC\-EXAMPLE\-BUCKET*/2021/01/01/example\-dataset\.csv
+ s3://*DOC\-EXAMPLE\-BUCKET*/2021/06/30/example\-dataset\.csv
+ s3://*DOC\-EXAMPLE\-BUCKET*/2021/12/31/example\-dataset\.csv

The datetime values within a relative time range change as time passes\. The S3 paths that fall within the relative time range might also differ\.

For the Amazon S3 file path, `s3://DOC-EXAMPLE-BUCKET1/20200101/example-dataset.csv`, `20220101` is an example of a path that can become a datetime parameter\.

To view a table of all parameters that you've created in Data Wrangler flow, choose the `\{\{\}\}` to the right of the text box containing the Amazon S3 path\. If you no longer need a parameter that you've created, you can edit or delete\. To edit or delete a parameter, choose icons to the right of the parameter\.

**Important**  
Before you delete a parameter, make sure that you haven't used it anywhere in your Data Wrangler flow\. Deleted parameters that are still within the flow cause errors\.

You can create parameters for any step of your Data Wrangler flow\. You can edit or delete any parameter that you create\. If the parameter has transformations that are no longer relevant to your use case, you can modify them\.

The following sections provide additional examples and general guidance on using parameters\. You can use the sections to understand the parameters that work best for you\.

**Note**  
The following sections contain procedures that use the Data Wrangler interface to override the parameters and create a processing job\.  
You can also override the parameters by using the following procedures\.  
To export your Data Wrangler flow and override the value of a parameter, do the following\.  
Choose the **\+** next to the node that you want to export\.
Choose **Export to**\.
Choose the location where you're exporting the data\.
Under `parameter_overrides`, specify different values for the parameters that you've created\.
Run the Jupyter Notebook\.

## Applying a Data Wrangler flow to files using patterns<a name="data-wrangler-pattern-parameters"></a>

You can use parameters to apply transformations in your Data Wrangler flow to different files that match a pattern in the Amazon S3 URI path\. This helps you specify the files in your S3 bucket that you want to transform with high specificity\. For example, you might have a dataset with the path `s3://DOC-EXAMPLE-BUCKET1/example-prefix-0/example-prefix-1/example-prefix-2/example-dataset.csv`\. Different datasets named `example-dataset.csv` are stored under many different example prefixes\. The prefixes might also be numbered sequentially\. You can create patterns for the numbers in the Amazon S3 URI\. Pattern parameters use REGEX to select any number of files that match the pattern of the expression\. The following are REGEX patterns that might be useful:
+ `.*` – Matches zero or more of any character, except newline characters
+ `.+` – Matches one or more of any character, excluding newline characters
+ `\d+` – Matches one or more of any decimal digit
+ `\w+` – Matches one or more of any alphanumeric character
+ `[abc-_]{2,4}` – Matches a string two, three, or four characters composed of the set of characters provided within a set of brackets
+ `abc|def` – Matches one string or another\. For example, the operation matches either `abc` or `def`

You can replace each number in the following paths with a single parameter that has a value of `\d+`\.
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix-3/example-prefix-4/example-prefix-5/example-dataset.csv`
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix-8/example-prefix-12/example-prefix-13/example-dataset.csv`
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix-4/example-prefix-9/example-prefix-137/example-dataset.csv`

The following procedure creates a pattern parameter for a dataset with the path `s3://DOC-EXAMPLE-BUCKET1/example-prefix-0/example-prefix-1/example-prefix-2/example-dataset.csv`\.

To create a pattern parameter, do the following\.

1. Next to the dataset that you've imported, choose **Edit dataset**\.

1. Highlight the `0` in `example-prefix-0`\.

1. Specify values for the following fields:
   + **Name** – A name for parameter
   + **Type** – **Pattern**
   + **Value** – **\\d\+** a regular expression that corresponds to one or more digits

1. Choose **Create**\.

1. Replace the `1` and the `2` in S3 URI path with the parameter\. The path should have the following format: `s3://DOC-EXAMPLE-BUCKET1/example-prefix-{{example_parameter_name}}/example-prefix-{{example_parameter_name}}/example-prefix-{{example_parameter_name}}/example-dataset.csv`

The following is a general procedure for creating a pattern parameter\.

1. Navigate to your Data Wrangler flow\.

1. Next to the dataset that you've imported, choose **Edit dataset**\.

1. Highlight the portion of the URI that you're using as the value of the pattern parameter\.

1. Choose **Create custom parameter**\.

1. Specify values for the following fields:
   + **Name** – A name for parameter
   + **Type** – **Pattern**
   + **Value** – A regular expression containing the pattern that you'd like to store\.

1. Choose **Create**\.

## Applying a Data Wrangler flow to files using numeric values<a name="data-wrangler-numeric-parameters"></a>

You can use parameters to apply transformations in your Data Wrangler flow to different files that have similar paths\. For example, you might have a dataset with the path `s3://DOC-EXAMPLE-BUCKET1/example-prefix-0/example-prefix-1/example-prefix-2/example-dataset.csv`\.

You might have the transformations from your Data Wrangler flow that you've applied to datasets under `example-prefix-1`\. You might want to apply the same transformations to `example-dataset.csv` that falls under `example-prefix-10` or `example-prefix-20`\.

You can create a parameter that stores the value `1`\. If you want to apply the transformations to different datasets, you can create processing jobs that replace the value of the parameter with a different value\. The parameter acts as a placeholder for you to change when you want to apply the transformations from your Data Wrangler flow to new data\. You can override the value of the parameter when you create a Data Wrangler processing job to apply the transformations in your Data Wrangler flow to different datasets\.

Use the following procedure to create numeric parameters for `s3://DOC-EXAMPLE-BUCKET1/example-prefix-0/example-prefix-1/example-prefix-2/example-dataset.csv`\.

To create parameters for the preceding S3 URI path, do the following\.

1. Navigate to your Data Wrangler flow\.

1. Next to the dataset that you've imported, choose **Edit dataset**\.

1. Highlight the number in an example prefix of `example-prefix-number`\.

1. Choose **Create custom parameter**\.

1. For **Name**, specify a name for the parameter\.

1. For **Type**, choose **Integer**\.

1. For **Value**, specify the number\.

1. Create parameters for the remaining numbers by repeating the procedure\.

After you've created the parameters, apply the transforms to your dataset and create a destination node for them\. For more information about destination nodes, see [Export](data-wrangler-data-export.md)\.

Use the following procedure to apply the transformations from your Data Wrangler flow to a different time range\. It assumes that you've created a destination node for the transformations in your flow\.

To change the value of a numeric parameter in a Data Wrangler processing job, do the following\.

1. From your Data Wrangler flow, choose **Create job**

1. Select only the destination node that contains the transformations to the dataset containing the datetime parameters\.

1. Choose **Configure job**\.

1. Choose **Parameters**\.

1. Choose the name of a parameter that you've created\.

1. Change the value of the parameter\.

1. Repeat the procedure for the other parameters\.

1. Choose **Run**\.

## Applying a Data Wrangler flow to files using strings<a name="data-wrangler-string-parameters"></a>

You can use parameters to apply transformations in your Data Wrangler flow to different files that have similar paths\. For example, you might have a dataset with the path `s3://DOC-EXAMPLE-BUCKET1/example-prefix/example-dataset.csv`\.

You might have transformations from your Data Wrangler flow that you've applied to datasets under `example-prefix`\. You might want to apply the same transformations to `example-dataset.csv` under `another-example-prefix` or `example-prefix-20`\.

You can create a parameter that stores the value `example-prefix`\. If you want to apply the transformations to different datasets, you can create processing jobs that replace the value of the parameter with a different value\. The parameter acts as a placeholder for you to change when you want to apply the transformations from your Data Wrangler flow to new data\. You can override the value of the parameter when you create a Data Wrangler processing job to apply the transformations in your Data Wrangler flow to different datasets\.

Use the following procedure to create a string parameter for `s3://DOC-EXAMPLE-BUCKET1/example-prefix/example-dataset.csv`\.

To create a parameter for the preceding S3 URI path, do the following\.

1. Navigate to your Data Wrangler flow\.

1. Next to the dataset that you've imported, choose **Edit dataset**\.

1. Highlight the example prefix, `example-prefix`\.

1. Choose **Create custom parameter**\.

1. For **Name**, specify a name for the parameter\.

1. For **Type**, choose **String**\.

1. For **Value**, specify the prefix\.

After you've created the parameter, apply the transforms to your dataset and create a destination node for them\. For more information about destination nodes, see [Export](data-wrangler-data-export.md)\.

Use the following procedure to apply the transformations from your Data Wrangler flow to a different time range\. It assumes that you've created a destination node for the transformations in your flow\.

To change the value of a numeric parameter in a Data Wrangler processing job, do the following:

1. From your Data Wrangler flow, choose **Create job**

1. Select only the destination node that contains the transformations to the dataset containing the datetime parameters\.

1. Choose **Configure job**\.

1. Choose **Parameters**\.

1. Choose the name of a parameter that you've created\.

1. Change the value of the parameter\.

1. Repeat the procedure for the other parameters\.

1. Choose **Run**\.

## Applying a Data Wrangler flow to different datetime ranges<a name="data-wrangler-datetime-parameters"></a>

Use datetime parameters to apply transformations in your Data Wrangler flow to different time ranges\. Highlight the portion of the Amazon S3 URI that has a timestamp and create a parameter for it\. When you create a parameter, you specify a time range from the current time to a time in the past\. For example, you might have an Amazon S3 URI that looks like the following: `s3://DOC-EXAMPLE-BUCKET1/example-prefix/2022/05/15/example-dataset.csv`\. You can save `2022/05/15` as a datetime parameter\. If you specify a year as the time range, the time range includes the moment that you run the processing job containing the datetime parameter and the time exactly one year ago\. If the moment you're running the processing job is September 6th, 2022 or `2022/09/06`, the time ranges can include the following:
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix/2022/03/15/example-dataset.csv`
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix/2022/01/08/example-dataset.csv`
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix/2022/07/31/example-dataset.csv`
+ `s3://DOC-EXAMPLE-BUCKET1/example-prefix/2021/09/07/example-dataset.csv`

The transformations in the Data Wrangler flow apply to all of the preceding prefixes\. Changing the value of the parameter in the processing job doesn't change the value of the parameter in the Data Wrangler flow\. To apply the transformations to datasets within a different time range, do the following:

1. Create a destination node containing all the transformations that you'd like to use\.

1. Create a Data Wrangler job\.

1. Configure the job to use a different time range for the parameter\. Changing the value of the parameter in the processing job doesn't change the value of the parameter in the Data Wrangler flow\.

For more information about destination nodes and Data Wrangler jobs, see [Export](data-wrangler-data-export.md)\.

The following procedure creates a datetime parameter for the Amazon S3 path: `s3://DOC-EXAMPLE-BUCKET1/example-prefix/2022/05/15/example-dataset.csv`\.

To create a datetime parameter for the preceding S3 URI path, do the following\.

1. Navigate to your Data Wrangler flow\.

1. Next to the dataset that you've imported, choose **Edit dataset**\.

1. Highlight the portion of the URI that you're using as the value of the datetime parameter\.

1. Choose **Create custom parameter**\.

1. For **Name**, specify a name for the parameter\.

1. For **Type**, choose **Datetime**\.
**Note**  
By default, Data Wrangler selects **Predefined**, which provides a dropdown menu for you to select a date format\. However, the timestamp format that you're using might not be available\. Instead of using **Predefined** as the default option, you can choose **Custom** and specify the timestamp format manually\.

1. For **Date format**, open the dropdown menu following **Predefined** and choose **yyyy/MM/dd**\. The format, **yyyy/MM/dd,** corresponds to the year/month/day of the timestamp\.

1. For **Timezone**, choose a time zone\.
**Note**  
The data that you're analyzing might have time stamps taken in a different time zone from your time zone\. Make sure that the time zone that you select matches the time zone of the data\. 

1. For **Time range**, specify the time range for the parameter\.

1. \(Optional\) Enter a description to describe how you're using the parameter\.

1. Choose **Create**\.

After you've created the datetime parameters, apply the transforms to your dataset and create a destination node for them\. For more information about destination nodes, see [Export](data-wrangler-data-export.md)\.

Use the following procedure to apply the transformations from your Data Wrangler flow to a different time range\. It assumes that you've created a destination node for the transformations in your flow\.

To change the value of a datetime parameter in a Data Wrangler processing job, do the following:

1. From your Data Wrangler flow, choose **Create job**

1. Select only the destination node that contains the transformations to the dataset containing the datetime parameters\.

1. Choose **Configure job**\.

1. Choose **Parameters**\.

1. Choose the name of the datetime parameter that you've created\.

1. For **Time range**, change the time range for the datasets\.

1. Choose **Run**\.