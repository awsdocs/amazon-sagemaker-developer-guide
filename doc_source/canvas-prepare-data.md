# Prepare data with advanced transformations<a name="canvas-prepare-data"></a>

Your machine learning dataset might require data preparation before you build your model\. You might want to clean your data due to various issues, which might include missing values or outliers, and perform feature engineering to improve the accuracy of your model\. Amazon SageMaker Canvas provides ML data transforms with which you can clean, transform, and prepare your data for model building\. You can use these transforms on your datasets without any code\. SageMaker Canvas adds the transforms you use to the **Model recipe**, which is a record of the data preparation done on your data before building the model\. Any data transforms you use only modify the input data for model building and do not modify your original data source\.

The following transforms are available in SageMaker Canvas for you to prepare your data for building\.

**Note**  
The preview of your dataset shows the first 100 rows of the dataset\. If your dataset has more than 20,000 rows, Canvas takes a random sample of 20,000 rows and previews the first 100 rows from that sample\. You can only search for and specify values from the previewed rows, and the filter functionality only filters the previewed rows and not the entire dataset\.

## Functions and operators<a name="canvas-prepare-data-custom-formula"></a>

You can use mathematical functions and operators to explore and distribute your data\. You can use the SageMaker Canvas supported functions or create your own formula with your existing data and create a new column with the result of the formula\. For example, you can add the corresponding values of two columns and save the result to a new column\.

You can nest statements to create more complex functions\. The following are some examples of nested functions that you might use\.
+ To calculate BMI, you could use the function `weight / (height ^ 2)`\.
+ To classify ages, you could use the function `Case(age < 18, 'child', age < 65, 'adult', 'senior')`\.

You can specify functions in the data preparation stage before you build your model\. To use a function, do the following\.
+ In the **Build** tab of the SageMaker Canvas app, choose **Functions** to open up the **Functions** panel\.
+ In the **Functions** panel, you can choose a **Formula** to add to your **Model Recipe**\. Each formula is applied to all of the values in the column\(s\) you specify\. For formulas that accept two or more columns as arguments, use columns with matching data types; otherwise, you will get an error or `null` values in the new column\. 
+ After you’ve specified a **Formula**, add a column name in the **New Column Name** field\. SageMaker Canvas uses this name for the new column that is created\.
+ To add the function to your Model Recipe, choose **Add**\.

SageMaker Canvas saves the result of your function to a new column using the name you specified in **New Column Name**\. You can view or remove functions from the **Model Recipe** panel\.

SageMaker Canvas supports the following operators for functions\. You can use either the text format or the in\-line format to specify your function\.


| Operator | Description | Supported data types | Text format | In\-line format | 
| --- | --- | --- | --- | --- | 
|  Add  |  Returns the sum of the values  |  Numeric  | Add\(sales1, sales2\) | sales1 \+ sales2 | 
|  Subtract  |  Returns the difference between the values  |  Numeric  | Subtract\(sales1, sales2\) | sales1 ‐ sales2 | 
|  Multiply  |  Returns the product of the values  |  Numeric  | Multiply\(sales1, sales2\) | sales1 \* sales2 | 
|  Divide  |  Returns the quotient of the values  |  Numeric  | Divide\(sales1, sales2\) | sales1 / sales2 | 
|  Mod  |  Returns the result of the modulo operator \(the remainder after dividing the two values\)  |  Numeric  | Mod\(sales1, sales2\) | sales1 % sales2 | 
|  Abs  | Returns the absolute value of the value |  Numeric  | Abs\(sales1\) | N/A | 
|  Negate  | Returns the negative of the value |  Numeric  | Negate\(c1\) | ‐c1 | 
|  Exp  |  Returns e \(Euler's number\) raised to the power of the value  |  Numeric  | Exp\(sales1\) | N/A | 
|  Log  |  Returns the logarithm \(base 10\) of the value  |  Numeric  | Log\(sales1\) | N/A | 
|  Ln  |  Returns the natural logarithm \(base e\) of the value  |  Numeric  | Ln\(sales1\) | N/A | 
|  Pow  |  Returns the value raised to a power  |  Numeric  | Pow\(sales1, 2\) | sales1 ^ 2 | 
|  If  |  Returns a true or false label based on a condition you specify  |  Boolean, Numeric, Text  | If\(sales1>7000, 'truelabel, 'falselabel'\) | N/A | 
|  Or  |  Returns a boolean value of whether one of the specified values/conditions is true or not  |  Boolean  | Or\(fullprice, discount\) | fullprice \|\| discount | 
|  And  |  Returns a boolean value of whether two of the specified values/conditions are true or not  |  Boolean  | And\(sales1,sales2\) | sales1 && sales2 | 
|  Not  |  Returns a boolean value that is the opposite of the specified value/conditions  |  Boolean  | Not\(sales1\) | \!sales1 | 
|  Case  |  Returns a boolean value based on conditional statements \(returns c1 if cond1 is true, returns c2 if cond2 is true, else returns c3\)  |  Boolean, Numeric, Text  | Case\(cond1, c1, cond2, c2, c3\) | N/A | 
|  Equal  |  Returns a boolean value of whether two values are equal  |  Boolean, Numeric, Text  | N/A | c1 = c2c1 == c2 | 
|  Not equal  |  Returns a boolean value of whether two values are not equal  |  Boolean, Numeric, Text  | N/A | c1 \!= c2 | 
|  Less than  |  Returns a boolean value of whether c1 is less than c2  |  Boolean, Numeric, Text  | N/A | c1 < c2 | 
|  Greater than  |  Returns a boolean value of whether c1 is greater than c2  |  Boolean, Numeric, Text  | N/A | c1 > c2 | 
|  Less than or equal  |  Returns a boolean value of whether c1 is less than or equal to c2  |  Boolean, Numeric, Text  | N/A | c1 <= c2 | 
|  Greater than or equal  |  Returns a boolean value of whether c1 is greater than or equal to c2  |  Boolean, Numeric, Text  | N/A | c1 >= c2 | 

SageMaker Canvas also supports aggregate operators, which can perform operations such as calculating the sum of all the values or finding the minimum value in a column\. You can use aggregate operators in combination with standard operators in your functions\. For example, to calculate the difference of values from the mean, you could use the function `Abs(height – avg(height))`\. SageMaker Canvas supports the following aggregate operators\.


| Aggregate operator | Description | Format | Example | 
| --- | --- | --- | --- | 
|  sum  |  Returns the sum of all the values in a column  | sum | sum\(c1\) | 
|  minimum  |  Returns the minimum value of a column  | min | min\(c2\) | 
|  maximum  |  Returns the maximum value of a column  | max | max\(c3\) | 
|  average  |  Returns the average value of a column  | avg | avg\(c4\) | 
|  std  | Returns the sample standard deviation of a column | std | std\(c1\) | 
|  stddev  | Returns the standard deviation of the values in a column | stddev | stddev\(c1\) | 
|  variance  | Returns the unbiased variance of the values in a column | variance | variance\(c1\) | 
|  approx\_count\_distinct  | Returns the approximate number of distinct items in a column | approx\_count\_distinct | approx\_count\_distinct\(c1\) | 
|  count  | Returns the number of items in a column | count | count\(c1\) | 
|  first  |  Returns the first value of a column  | first | first\(c1\) | 
|  last  |  Returns the last value of a column  | last | last\(c1\) | 
|  stddev\_pop  | Returns the population standard deviation of a column | stddev\_pop | stddev\_pop\(c1\) | 
|  variance\_pop  |  Returns the population variance of the values in a column  | variance\_pop | variance\_pop\(c1\) | 

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

## Replace values<a name="canvas-prepare-data-replace"></a>

This transform replaces values in your dataset where the values in a specific column meet conditions that you specify\. You can replace missing values or outliers\. SageMaker Canvas uses the replaced values when building your model but doesn’t change your original dataset\. Note that if you've dropped a column from your dataset using the [Drop columns](#canvas-prepare-data-drop) transform, you can't replace values in that column\.

### Replace missing values<a name="canvas-prepare-data-replace-missing"></a>

Missing values are a common occurrence in machine learning datasets and can impact model accuracy\. You can choose to drop rows that have missing values, but your model is more accurate if you choose to replace the missing values instead\. With this transform, you can replace missing values in numeric columns with the mean or median of the data in a column, or you can also specify a custom value with which to replace missing values\. For non\-numeric columns, you can replace missing values with the mode \(most common value\) of the column or a custom value\.

Use this transform if you want to replace the null or empty values in certain columns\. To replace missing values in a specified column, do the following\. 

1. In the **Build** tab of the SageMaker Canvas application, choose **Replace**\.

1. Choose the **Column** in which you want to replace missing values\.

1. For **Values to replace**, choose **Is missing**\.

1. Set **Mode** to either **Automatic \(default\)** or **Manual**\. If you choose **Automatic \(default\)**, SageMaker Canvas replaces missing values with imputed values that best fit your data\. If you choose **Manual**, then specify the **Replace with** value in the next step\.

1. \(Optional\) If you choose the **Manual** replacement option, set the **Replace with** value: 
   + If your column is numeric, then select **Mean**, **Median**, or **Custom**\. **Mean** replaces missing values with the mean for the column, and **Median** replaces missing values with the median for the column\. If you choose **Custom**, then you must specify a custom value that you want to use to replace missing values\.
   + If your column is non\-numeric, then select **Mode** or **Custom**\. **Mode** replaces missing values with the mode, or the most common value, for the column\. For **Custom**, specify a custom value\. that you want to use to replace missing values\.

1. Choose **Add** to add the transform to the **Model recipe**\.

After replacing the missing values in the dataset, SageMaker Canvas adds the transform in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the missing values return to the dataset\.

![\[Screenshot of the replace missing values operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-replace-missing.png)

### Replace outliers<a name="canvas-prepare-data-replace-outliers"></a>

Outliers, or rare values in the distribution and range of your data, can negatively impact model accuracy and lead to longer building times\. SageMaker Canvas enables you to detect outliers in numeric columns and replace the outliers with values that lie within an accepted range in your data\. You can choose to define outliers with either standard deviations or a custom range, and you can replace outliers with the minimum and maximum values in the accepted range\.

To replace outliers in your data, do the following\.

1. In the **Build** tab of the SageMaker Canvas application, choose **Replace**\.

1. Choose the **Column** in which you want to replace outliers\.

1. For **Values to replace**, choose **Is outlier**\.

1. For **Define outliers**, choose either **Standard deviation** or **Custom Range**\.

1. If you choose **Standard deviation**, specify a **SD** \(standard deviation\) value from 1–3\. If you choose **Custom Range**, select either **Percentile** or **Number**, and then specify the **Min** and **Max** values\.

1. For **Replace with**, select **Min/max range**\.

1. Choose **Add** to add the transform to the **Model recipe**\.

The **Standard deviation** option detects outliers in numeric columns using the mean and standard deviation\. You specify the number of standard deviations a value must vary from the mean to be considered an outlier\. For example, if you specify 3 for **SD**, a value must fall more than 3 standard deviations from the mean to be considered an outlier\. SageMaker Canvas replaces outliers with the minimum value or maximum value in the accepted range\. For example, if you configure the standard deviations to only include values from 200 to 300, then SageMaker Canvas changes a value of 198 to 200 \(the minimum\)\.

The **Custom Range** option detects outliers in numeric columns using minimum and maximum values\. Use this method if you know your threshold values that delimit outliers\. You can set the **Type** of the custom range to either **Percentile** or **Number**\. If you choose **Percentile**, the **Min** and **Max** values should be the minimum and maximum of the percentile range \(0\-100\) that you want to allow\. If you choose **Number**, the **Min** and **Max** values should be the minimum and maximum numeric values that you want to allow\. SageMaker Canvas replaces any values that fall outside of the minimum and maximum to the minimum and maximum values\. For example, if your range only allows values from 1 to 100, then SageMaker Canvas changes a value of 102 to 100 \(the maximum\)\.

After replacing the values in the dataset, SageMaker Canvas adds the transform in the **Model recipe** section\. If you remove the transform from the **Model recipe** section, the original values return to the dataset\.

![\[Screenshot of the replace outliers operation in the SageMaker Canvas application.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-replace-outlier.png)

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