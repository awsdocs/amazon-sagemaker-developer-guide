# Prepare data with advanced transformations<a name="canvas-prepare-data"></a>

Your machine learning dataset might require data preparation before you build your model\. You might want to clean your data due to various issues, which might include missing values or outliers, and perform feature engineering to improve the accuracy of your model\. Amazon SageMaker Canvas provides ML data transforms with which you can clean, transform, and prepare your data for model building\. You can use these transforms on your datasets without any code\. SageMaker Canvas adds the transforms you use to the **Model recipe**, which is a record of the data preparation done on your data before building the model\. Any data transforms you use only modify the input data for model building and do not modify your original data source\.

The following transforms are available in SageMaker Canvas for you to prepare your data for building\.

**Note**  
The preview of your dataset shows the first 100 rows of the dataset\. If your dataset has more than 20,000 rows, Canvas takes a random sample of 20,000 rows and previews the first 100 rows from that sample\. You can only search for and specify values from the previewed rows, and the filter functionality only filters the previewed rows and not the entire dataset\.

## Datetime extraction<a name="canvas-prepare-data-datetime"></a>

With the datetime extraction transform, you can extract values from a datetime column to a separate column\. For example, if you have a column containing dates of purchases, you can extract the month value to a separate column and use the new column when building your model\. You can also extract multiple values to separate columns with a single transform\.

Your datetime column must use a supported timestamp format\. For a list of the formats that SageMaker Canvas supports, see [Time Series Forecasts in Amazon SageMaker Canvas](canvas-time-series.md)\. If your dataset does not use one of the supported formats, update your dataset to use a supported timestamp format and re\-import it to Amazon SageMaker Canvas before building your model\.

To perform a datetime extraction, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Extract**\.

1. Choose the **Column** from which you want to extract values\.

1. For **Value**, select one or more values to extract from the column\. The values you can extract from a timestamp column are **Year**, **Month**, **Day**, **Hour**, **Week of year**, **Day of year**, and **Quarter**\.

1. Choose **Add** to add the transform to the **Model recipe**\.

SageMaker Canvas creates a new column in the dataset for each of the values you extract\. Except for **Year** values, SageMaker Canvas uses a 0\-based encoding for the extracted values\. For example, if you extract the **Month** value, January is extracted as 0, and February is extracted as 1\.

![\[Screenshot of the datetime extraction box in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-datetime-extract.png)

You can see the transform listed in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the new columns are removed from the dataset\.

## Drop columns<a name="canvas-prepare-data-drop"></a>

You can exclude a column from your model build by dropping it in the **Build** tab of the SageMaker Canvas application\. Deselect the column you want to drop, and it isn't included when building the model\.

**Note**  
If you drop columns and then make [batch predictions](canvas-getting-started.md#canvas-getting-started-step5) with your model, SageMaker Canvas adds the dropped columns back to the \.csv file available for you to download\. However, SageMaker Canvas does not add the dropped columns back for time series models\.

## Rename columns<a name="canvas-prepare-data-rename"></a>

With the rename columns transform, you can rename columns in your data\. When you rename a column, SageMaker Canvas changes the column name in the model input\.

You can rename a column in your dataset by double\-clicking on the column name in the **Build** tab of the SageMaker Canvas application and entering a new name\. Pressing the **Enter** key submits the change, and clicking anywhere outside the input cancels the change\. You can also rename a column by clicking the **More options** icon \(![\[More options icon at the end of a row.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/more-options-icon.png)\), located at the end of the row in list view or at the end of the header cell in grid view, and choosing **Rename**\.

Your column name can’t be longer than 32 characters or have double underscores \(\_\_\), and you can’t rename a column to the same name as another column\. You also can’t rename a dropped column\.

The following screenshot shows how to rename a column by double\-clicking the column name\.

![\[Screenshot of renaming a column with the double-click method in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-rename-column.png)

When you rename a column, SageMaker Canvas adds the transform in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the column reverts to its original name\.

## Remove rows<a name="canvas-prepare-data-remove"></a>

This transform removes rows of data from the dataset where values in a specific column meet conditions that you specify\. You can remove rows that have missing values, contain outliers, or meet custom conditions in a column you choose\. These rows are not used when building your model\.

### Remove rows by missing values<a name="canvas-prepare-data-remove-missing"></a>

Missing values are a common occurrence in machine learning datasets and can impact model accuracy\. Use this transform if you want to drop rows with null or empty values in certain columns\.

To remove rows that contain missing values in a specified column, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Remove rows by**\.

1. Choose the **Column** you want to check for missing values\.

1. For the **Operation**, choose **Is missing**\.

1. Choose **Add** to add the transform to the **Model recipe**\.

SageMaker Canvas drops rows that contain missing values in the **Column** you selected\. After removing the rows from the dataset, SageMaker Canvas adds the transform in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the rows return to your dataset\.

![\[Screenshot of the remove rows by missing values operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-remove-missing.png)

### Remove rows by outliers<a name="canvas-prepare-data-remove-outliers"></a>

Outliers, or rare values in the distribution and range of your data, can negatively impact model accuracy and lead to longer building times\. With SageMaker Canvas, you can detect and remove rows that contain outliers in numeric columns\. You can choose to define outliers with either standard deviations or a custom range\.

To remove outliers from your data, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Remove rows by**\.

1. Choose the **Column** you want to check for outliers\.

1. For the **Operation**, choose **Is outlier**\.

1. Set the **Outlier range** to either **Standard deviation** or **Custom range**\.

1. If you choose **Standard deviation**, specify a **SD** \(standard deviation\) value from 1–3\. If you choose **Custom range**, select either **Percentile** or **Number**, and then specify the **Min** and **Max** values\.

1. Choose **Add** to add the transform to the **Model recipe**\.

The **Standard deviation** option detects and removes outliers in numeric columns using the mean and standard deviation\. You specify the number of standard deviations a value must vary from the mean to be considered an outlier\. For example, if you specify `3` for **SD**, a value must fall more than 3 standard deviations from the mean to be considered an outlier\.

The **Custom range** option detects and removes outliers in numeric columns using minimum and maximum values\. Use this method if you know your threshold values that delimit outliers\. You can set the **Type** of the range to either **Percentile** or **Number**\. If you choose **Percentile**, the **Min** and **Max** values should be the minimum and maximum of the percentile range \(0–100\) that you want to allow\. If you choose **Number**, the **Min** and **Max** values should be the minimum and maximum numeric values that you want to allow in the data\.

After removing the rows from the dataset, SageMaker Canvas adds the transform in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the rows return to your dataset\.

![\[Screenshot of the remove rows by outliers operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-remove-outlier.png)

### Remove rows by custom values<a name="canvas-prepare-data-remove-custom"></a>

You can remove rows with values that meet custom conditions\. For example, you might want to exclude all of the rows with a price value greater than 100 when building your model\. With this transform, you can create a rule that removes all rows that exceed the threshold you set\.

To use the custom remove transform, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Remove rows by**\.

1. Choose the **Column** you want to check\.

1. Select the type of **Operation** you want to use, and then specify the value\(s\) for the selected condition\.

1. Choose **Add** to add the transform to the **Model recipe**\.

For the **Operation**, you can choose one of the following options\. Note that the available operations depend on the data type of the column you choose\. For example, you cannot create a “Greater than” operation for a column containing text values\.


| Operation | Supported column type | Function | 
| --- | --- | --- | 
|  Is equal to  |  Binary, numeric, text, categorical  |  Removes rows where the value in **Column** equals the values you specify\.  | 
|  Is not equal to  |  Binary, numeric, text, categorical  |  Removes rows where the value in **Column** doesn't equal the values you specify\.  | 
|  Is less than  |  Numeric  |  Removes rows where the value in **Column** is less than the value you specify\.  | 
|  Is less than or equal to  |  Numeric  |  Removes rows where the value in **Column** is less than or equal to the value you specify\.  | 
|  Is greater than  |  Numeric  |  Removes rows where the value in **Column** is greater than the value you specify\.  | 
|  Is greater than or equal to  | Numeric |  Removes rows where the value in **Column** is greater than or equal to the value you specify\.  | 
|  Is between  | Numeric |  Removes rows where the value in **Column** is between or equal to two values you specify\.  | 
|  Contains  |  Text, categorical  |  Removes rows where the value in **Column** contains a values you specify\.  | 
|  Starts with  |  Text, categorical  |  Removes rows where the value in **Column** begins with a value you specify\.  | 
|  Ends with  |  Text, categorical  |  Removes rows where the value in **Column** ends with a value you specify\.  | 

After removing the rows from the dataset, SageMaker Canvas adds the transform in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the rows return to your dataset\.

![\[Screenshot of the remove rows by custom values operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-remove-custom.png)

## Filter rows<a name="canvas-prepare-data-filter"></a>

The filter functionality filters the previewed rows \(the first 100 rows of your dataset\) according to conditions that you specify\. Filtering rows creates a temporary preview of the data and does not impact the model building\. You can filter to preview rows that have missing values, contain outliers, or meet custom conditions in a column you choose\.

### Filter rows by missing values<a name="canvas-prepare-data-filter-missing"></a>

Missing values are a common occurrence in machine learning datasets\. If you have rows with null or empty values in certain columns, you might want to filter for and preview those rows\.

To filter missing values from your previewed data, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Filter by rows ** \(![\[Filter icon in the SageMaker Canvas app.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/filter-icon.png)\)\.

1. Choose the **Column** you want to check for missing values\.

1. For the **Operation**, choose **Is missing**\.

SageMaker Canvas filters for rows that contain missing values in the **Column** you selected and provides a preview of the filtered rows\.

![\[Screenshot of the filter by missing values operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-filter-missing.png)

### Filter rows by outliers<a name="canvas-prepare-data-filter-outliers"></a>

Outliers, or rare values in the distribution and range of your data, can negatively impact model accuracy and lead to longer building times\. SageMaker Canvas enables you to detect and filter rows that contain outliers in numeric columns\. You can choose to define outliers with either standard deviations or a custom range\.

To filter for outliers in your data, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Filter by rows ** \(![\[Filter icon in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/filter-icon.png)\)\.

1. Choose the **Column** you want to check for outliers\.

1. For the **Operation**, choose **Is outlier**\.

1. Set the **Outlier range** to either **Standard deviation** or **Custom range**\.

1. If you choose **Standard deviation**, specify a **SD** \(standard deviation\) value from 1–3\. If you choose **Custom range**, select either **Percentile** or **Number**, and then specify the **Min** and **Max** values\.

The **Standard deviation** option detects and filters for outliers in numeric columns using the mean and standard deviation\. You specify the number of standard deviations a value must vary from the mean to be considered an outlier\. For example, if you specify `3` for **SD**, a value must fall more than 3 standard deviations from the mean to be considered an outlier\.

The **Custom range** option detects and filters for outliers in numeric columns using minimum and maximum values\. Use this method if you know your threshold values that delimit outliers\. You can set the **Type** of the range to either **Percentile** or **Number**\. If you choose **Percentile**, the **Min** and **Max** values should be the minimum and maximum of the percentile range \(0\-100\) that you want to allow\. If you choose **Number**, the **Min** and **Max** values should be the minimum and maximum numeric values that you want to filter in the data\.

![\[Screenshot of the filter by outliers operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-filter-outlier.png)

### Filter rows by custom values<a name="canvas-prepare-data-filter-custom"></a>

You can filter for rows with values that meet custom conditions\. For example, you might want to preview rows that have a price value greater than 100 before removing them\. With this functionality, you can filter rows that exceed the threshold you set and preview the filtered data\.

To use the custom filter functionality, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Filter by rows** \(![\[Filter icon in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/filter-icon.png)\)\.

1. Choose the **Column** you want to check\.

1. Select the type of **Operation** you want to use, and then specify the value\(s\) for the selected condition\.

For the **Operation**, you can choose one of the following options\. Note that the available operations depend on the data type of the column you choose\. For example, you cannot create a “Greater than” operation for a column containing text values\.


| Operation | Supported column type | Function | 
| --- | --- | --- | 
|  Is equal to  |  Binary, numeric, text, categorical  |  Filters rows where the value in **Column** equals the values you specify\.  | 
|  Is not equal to  |  Binary, numeric, text, categorical  |  Filters rows where the value in **Column** doesn't equal the values you specify\.  | 
|  Is less than  |  Numeric  |  Filters rows where the value in **Column** is less than the value you specify\.  | 
|  Is less than or equal to  |  Numeric  |  Filters rows where the value in **Column** is less than or equal to the value you specify\.  | 
|  Is greater than  |  Numeric  |  Filters rows where the value in **Column** is greater than the value you specify\.  | 
|  Is greater than or equal to  | Numeric |  Filters rows where the value in **Column** is greater than or equal to the value you specify\.  | 
|  Is between  | Numeric |  Filters rows where the value in **Column** is between or equal to two values you specify\.  | 
|  Contains  |  Text, categorical  |  Filters rows where the value in **Column** contains a values you specify\.  | 
|  Starts with  |  Text, categorical  |  Filters rows where the value in **Column** begins with a value you specify\.  | 
|  Ends with  |  Text, categorical  |  Filters rows where the value in **Column** ends with a value you specify\.  | 

After you set the filter operation, SageMaker Canvas updates the preview of the dataset to show you the filtered data\.

![\[Screenshot of the filter by custom values operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-filter-custom.png)