# Transform Data<a name="data-wrangler-transform"></a>

Amazon SageMaker Data Wrangler provides numerous ML data transforms to streamline cleaning, transforming, and featurizing your data\. When you add a transform, it adds a step to the data flow\. Each transform you add modifies your dataset and produces a new dataframe\. All subsequent transforms apply to the resulting dataframe\. 

Data Wrangler includes built\-in transforms, which you can use to transform columns without any code\. You can also add custom transformations using PySpark, Pandas, and PySpark SQL\. Some transforms operate in place, while others create a new output column in your dataset\. 

Use this page to learn more about these built\-in and custom transforms\.

## Transform UI<a name="data-wrangler-transform-ui"></a>

Most of the built\-in transforms are located in the **Prepare** tab of the Data Wrangler UI\. The Join and Concatenate transforms are accessed through the data flow view\. Use the following table to preview these two views\. 

------
#### [ Join View ]

To join two datasets, select the first dataset in your data flow and choose **Join**\. When you select **Join**, you see results similar to those shown in the following image\. Your left and right datasets are displayed in the left panel\. The main panel displays your data flow, with the newly joined dataset added\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/join-1.png)

When you select **Configure** to configure your join, you see results similar to those shown in the following image\. Your join configuration is display in the left panel\. You can use this panel to choose the joined dataset name, join type, and columns to join on\. The main panel displays three tables\. The top\-two tables display the Left and Right datasets on the left and right respectively\. Under this table, you can preview the joined dataset\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/join-2.png)

See [Join Datasets](#data-wrangler-transform-join) to learn more\. 

------
#### [ Concatenate View ]

To concatenate two datasets, you select the first dataset in your data flow and choose **Concatenate**\. When you select **Concatenate**, you see results similar to those shown in the following image\. Your left and right datasets are displayed in the left panel\. The main panel displays your data flow, with the newly concatenated dataset added\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/concat-1.png)

When you select **Configure** to configure your concatenation, you see results similar to those shown in the following image\. Your concatenate configuration is display in the left panel\. You can use this panel to choose the concatenated dataset's name, and choose to remove duplicates after concatenation and add columns to indicate the source dataframe\. The main panel displays three tables\. The top\-two tables display the Left and Right datasets on the left and right respectively\. Under this table, you can preview the concatenated dataset\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/concat-2.png)

See [Concatenate Datasets](#data-wrangler-transform-concatenate) to learn more\.

------
#### [ Transforms in the Prepare Tab ]

To access transforms on the **Prepare** tab, select **\+** next to a step in your data flow, and select **Add transform**\.

On the **Prepare** tab, you add steps under **Add**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/prepare-list.png)

You can use the **Previous steps** tab to view and remove transformations that you have added, in sequential order\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/prepare-list-multiple.png)

------

## Join Datasets<a name="data-wrangler-transform-join"></a>

You join dataframes directly in your data flow\. When you join two datasets, the resulting joined dataset appears in your flow\. The following join types are supported by Data Wrangler\.
+ **Left Outer** – Include all rows from the left table\. If the value for the column joined on in a left table row does not match any right table row values, that row contains null values for all right table columns in the joined table\.
+ **Left Anti** – Include rows from the left table that do not contain values in the right table for the joined column\.
+ **Left semi** – Include a single row from the left table for all identical rows that satisfy the criteria in the join statement\. This excludes duplicate rows from the left table that match the criteria of the join\.
+ **Right Outer** – Include all rows from the right table\. If the value for the joined column in a right table row does not match any left table row values, that row contains null values for all left table columns in the joined table\.
+ **Inner** – Include rows from left and right tables that contain matching values in the joined column\. 
+ **Full Outer** – Include all rows from the left and right tables\. If the row value for the joined column in either table does not match, separate rows are created in the joined table\. If a row doesn’t contain a value for a column in the joined table, null is inserted for that column\.
+ **Cartesian Cross** – Include rows which combine each row from the first table with each row from the second table\. This is a [Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) of rows from tables in the join\. The result of this product is the size of the left table times the size of the right table\. Therefore, we recommend caution in using this join between very large datasets\. 

Use the following procedure to join two dataframes\.

1. Select **\+** next to the left dataframe that you want to join\. The first dataframe you select is always the left table in your join\. 

1. Select **Join**\.

1. Select the right dataframe\. The second dataframe you select is always the right table in your join\.

1. Select **Configure** to configure your join\. 

1. Give your joined dataset a name using the **Name** field\.

1. Select a **Join type**\.

1. Select a column from the left and right tables to join on\. 

1. Select **Apply** to preview the joined dataset on the right\. 

1. To add the joined table to your data flow, select **Add** in the top right corner\. 

## Concatenate Datasets<a name="data-wrangler-transform-concatenate"></a>

**Concatenate two datasets:**

1. Select **\+** next to the left dataframe that you want to concatenate\. The first dataframe you select is always the left table in your concatenate\. 

1. Select **Concatenate**\.

1. Select the right dataframe\. The second dataframe you select is always the right table in your concatenate\.

1. Select **Configure** to configure your concatenate\. 

1. Give your concatenated dataset a name using the **Name** field\.

1. \(Optional\) Select the check box next to **Remove duplicates after concatenation** to remove duplicate columns\. 

1. \(Optional\) Select the check box next to **Add column to indicate source dataframe** if, for each column in the new dataset, you want to add an indicator of the column's source\. 

1. Select **Apply** to preview the new dataset\. 

1. Select **Add** to add the new dataset to your data flow\. 

## Custom Transforms<a name="data-wrangler-transform-custom"></a>

The **Custom Transforms** group allows you to use Pyspark, Pandas, or Pyspark \(SQL\) to define custom transformations\. For all three options, you use the variable `df` to access the dataframe to which you want to apply the transform\. You do not need to include a return statement\. Select **Preview** to preview the result of the custom transform\. Select **Add** to add the custom transform to your list of **Previous steps**\.

You can import the popular libraries with an `import` statement in the custom transform code block such as the following:
+ Numpy version 1\.19\.0
+ Scikit\-learn version 0\.23\.2
+ Scipy version 1\.5\.4
+ Pandas version 1\.0\.3
+ Pyspark version 3\.0\.0

If you include print statements in the code block, the result appears when you select **Preview**\.

The following are examples of how you can use the custom transforms code block:

**Pyspark**

The following example extracts date and time from a timestamp\.

```
from pyspark.sql.functions import from_unixtime, to_date, date_format
df = df.withColumn('DATE_TIME', from_unixtime('TIMESTAMP'))
df = df.withColumn( 'EVENT_DATE', to_date('DATE_TIME')).withColumn(
'EVENT_TIME', date_format('DATE_TIME', 'HH:mm:ss'))
```

**Pandas**

The following example provides an overview of the dataframe to which you are adding transforms\. 

```
df.info()
```

**Pyspark \(SQL\)**

The following creates a new dataframe with five columns: *name*, *fare*, *pclass*, *survived*\.

```
SELECT name, fare, pclass, survived FROM df
```

## Custom Formula<a name="data-wrangler-transform-custom-formula"></a>

Use **Custom formula** to define a new column using a Spark SQL expression to query data in the current dataframe\. The query must use the conventions of Spark SQL expressions\.

**Important**  
**Custom formula** does not support columns with spaces in the name\. You can use the **Rename column** transform in the **Manage columns** transform group to remove spaces from a column's name\. You can also add a **Pandas** **Custom transform** similar to the following to remove spaces from multiple columns in a single step\. This example changes columns named `A column` and `B column` to `A_column` and `B_column` respectively\.   

```
df.rename(columns={"A column": "A_column", "B column": "B_column"})
```

You can use this transform to perform operations on columns, referencing the columns by name\. For example, assuming the current dataframe contains columns named *col\_a* and *col\_b*, you can use the following operation to produce an **Output column** that is the product of these two columns with the following code:

```
col_a * col_b
```

Other common operations include the following, assuming a dataframe contains `col_a` and `col_b` columns:
+ Concatenate two columns: `concat(col_a, col_b)`
+ Add two columns: `col_a + col_b`
+ Subtract two columns: `col_a - col_b`
+ Divide two columns: `col_a / col_b`
+ Take the absolute value of a column: `abs(col_a)`

For more information, see the [Spark documentation](http://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=regex#pyspark.sql.DataFrame.selectExpr) on selecting data\. 

## Encode Categorical<a name="data-wrangler-transform-cat-encode"></a>

Categorical data is usually composed of a finite number of categories, where each category is represented with a string\. For example, if you have a table of customer data, a column that indicates the country a person lives in is categorical\. The categories would be Afghanistan, Albania, Algeria, and so on\. Categorical data can be *nominal* or *ordinal*\. Ordinal categories have an inherent order, and nominal categories do not\. The highest degree obtained \(High school, Bachelors, Master\) is an example of ordinal categories\. 

Encoding categorical data is the process of creating a numerical representation for categories\. For example, if your categories are Dog and Cat, you may encode this information into two vectors, `[1,0]` to represent dog, and `[0,1]` to represent cat\.

When you encode ordinal categories, you may need to translate the natural order of categories into your encoding\. For example, you can represent highest degree obtained with the following map: `{"High school": 1, "Bachelors": 2, "Masters":3}`\.

Use categorical encoding to encode categorical data that is in string format into arrays of integers\. 

The Data Wrangler categorical encoders create encodings for all categories that exist in a column at the time the step is defined\. If new categories have been added to a column when you start a Data Wrangler job to process your dataset at time *t*, and this column was the input for a Data Wrangler categorical encoding transform at time *t*\-1, these new categories are considered *missing* in the Data Wrangler job\. The option you select for **Invalid handling strategy** is applied to these missing values\. Examples of when this can occur are: 
+ When you use a \.flow file to create a Data Wrangler job to process a dataset that was updated after the creation of the data flow\. For example, you may use a data flow to regularly process sales data each month\. If that sales data is updated weekly, new categories may be introduced into columns for which an encode categorical step is defined\. 
+ When you select **Sampling** when you import your dataset, some categories may be left out of the sample\. 

In these situations, these new categories are considered missing values in the Data Wrangler job\.

You can choose from and configure an *ordinal* and a *one\-hot encode*\. Use the following sections to learn more about these options\. 

Both transforms create a new column named **Output column name**\. You specify the output format of this column with **Output style**:
+ Select **Vector** to produce a single column with a sparse vector\. 
+ Select **Columns** to create a column for every category with an indicator variable for whether the text in the original column contains a value that is equal to that category\.

### Ordinal Encode<a name="data-wrangler-transform-cat-encode-ordinal"></a>

Select **Ordinal encode** to encode categories into an integer between 0 and the total number of categories in the **Input column** you select\.

**Invalid handing strategy**: Select a method to handle invalid or missing values\. 
+ Choose **Skip** if you want to omit the rows with missing values\.
+ Choose **Keep** to retain missing values as the last category\.
+ Choose **Error** if you want Data Wrangler to throw an error if missing values are encountered in the **Input column**\.
+ Choose **Replace with NaN** to replace missing with NaN\. This option is recommended if your ML algorithm can handle missing values\. Otherwise, the first three options in this list may produce better results\.

### One\-Hot Encode<a name="data-wrangler-transform-cat-encode-onehot"></a>

Select **One\-hot encode** for **Transform** to use one\-hot encoding\. Configure this transform using the following: 
+ **Drop last category**: If `True`, the last category does not have a corresponding index in the one\-hot encoding\. When missing values are possible, a missing category is always the last one and setting this to `True` means that a missing value results in an all zero vector\.
+ **Invalid handing strategy**: Select a method to handle invalid or missing values\. 
  + Choose **Skip** if you want to omit the rows with missing values\.
  + Choose **Keep** to retain missing values as the last category\.
  + Choose **Error** if you want Data Wrangler to throw an error if missing values are encountered in the **Input column**\.
+ **Is input ordinal encoded**: Select this option if the input vector contains ordinal encoded data\. This option requires that input data contain non\-negative integers\. If **True**, input *i* is encoded as a vector with a non\-zero in the *i*th location\. 

## Featurize Text<a name="data-wrangler-transform-featurize-text"></a>

Use the **Feature Text** transform group to inspect string typed columns and use text embedding to featurize these columns\. 

This feature group contains two features, *Character statistics* and *Vectorize*\. Use the following sections to learn more about these transforms\. For both options, the **Input column** must contain text data \(string type\)\.

### Character Statistics<a name="data-wrangler-transform-featurize-text-character-stats"></a>

Use **Character statistics** to generate statistics for each row in a column containing text data\. 

This transforms computes the following ratios and counts for each row, and creates a new column to report the result\. The new column is named using the input column name as a prefix and a suffix that is specific to the ratio or count\. 
+ Number of words: The total number of words in that row\. The suffix for this output column is **\-stats\_word\_count**\.
+ Number of characters: The total number of characters in that row\. The suffix for this output column is **\-stats\_char\_count**\.
+ Ratio of upper: The number of upper\-case characters, from A to Z, divided by all characters in the column\. The suffix for this output column is **\-stats\_capital\_ratio**\.
+ Ratio of lower: The number of lower\-case characters, from a to z, divided by all characters in the column\. The suffix for this output column is **\-stats\_lower\_ratio**\.
+ Ratio of digits: The ratio of digits in a single row over the sum of digits in the input column\. The suffix for this output column is **\-stats\_digit\_ratio**\.
+ Special characters ratio: The ratio of non\-alphanumeric \(characters like \#$&%:@\) characters to over the sum of all characters in the input column\. The suffix for this output column is **\-stats\_special\_ratio**\.

### Vectorize<a name="data-wrangler-transform-featurize-text-vectorize"></a>

Text embedding involves mapping words or phrases from a vocabulary to vectors of real numbers\. Use the Data Wrangler text embedding transform to tokenize and vectorize text data into term frequency–inverse document frequency \(TF\-IDF\) vectors\. 

When TF\-IDF is calculated for a column of text data, each word in each sentence is converted to a real number that represents its semantic importance\. Higher numbers are associated with less frequent words, which tend to be more meaningful\. 

When you define a **Vectorize** transform step, the count vectorizer and TF\-IDF methods are defined using data available in Data Wrangler when defining this step\. These same methods will be used when running a Data Wrangler job\.

You configure this transform using the following: 
+ **Output column name**: This transform will create a new column with the text embedding\. Use this field to specify a name for this output column\. 
+ **Tokenizer**: A tokenizer converts the sentence into a list of words, or *tokens*\. 

  Choose **Standard** to use a tokenizer that splits by white space and converts each word to lowercase\. For example, `"Good dog"` is tokenized to `["good","dog"]`\.

  Choose **Custom** to use a customized tokenizer\. If you choose **Custom**, you can use the following fields to configure the tokenizer:
  + **Minimum token length**: The minimum length, in characters, for a token to be valid\. Defaults to `1`\. For example, if you specify `3` for minimum token length, words like `a, at, in` are dropped from the tokenized sentence\. 
  + **Should regex split on gaps**: If selected, **regex** splits on gaps\. Otherwise, it matches tokens\. Defaults to `True`\. 
  + **Regex pattern**: Regex pattern that defines the tokenization process\. Defaults to `' \\ s+'`\.
  + **To lowercase**: If chosen, all characters are converted to lowercase before tokenization\. Defaults to `True`\.

  To learn more, refer to the Spark documentation on [Tokenizer](https://spark.apache.org/docs/3.0.0/ml-features#tokenizer)\.
+ **Vectorizer**: The vectorizer converts the list of tokens into a sparse numeric vector\. Each token corresponds to an index in the vector and a non\-zero indicates the existence of the token in the input sentence\. You can choose from two vectorizer options, *Count* and *Hashing*\.
  + **Count vectorize** allows customizations that filter infrequent or too common tokens\. **Count vectorize parameters** include the following: 
    + **Minimum term frequency**: In each row, terms \(tokens\) with smaller frequency are filtered\. If you specify an integer, this is an absolute threshold \(inclusive\)\. If you specify a fraction between 0 \(inclusive\) and 1, the threshold is relative to the total term count\. Defaults to `1`\.
    + **Minimum document frequency**: Minimum number of rows in which a term \(token\) must appear to be included\. If you specify an integer, this is an absolute threshold \(inclusive\)\. If you specify a fraction between 0 \(inclusive\) and 1, the threshold is relative to the total term count\. Defaults to `1`\.
    + **Maximum document frequency**: Maximum number of documents \(rows\) in which a term \(token\) can appear to be included\. If you specify an integer, this is an absolute threshold \(inclusive\)\. If you specify a fraction between 0 \(inclusive\) and 1, the threshold is relative to the total term count\. Defaults to `0.999`\.
    + **Maximum vocabulary size**: Maximum size of the vocabulary\. The vocabulary is made up of all terms \(tokens\) in all rows of the column\. Defaults to `262144`\.
    + **Binary outputs**: If selected, the vector outputs do not include the number of appearances of a term in a document, but rather are a binary indicator of its appearance\. Defaults to `False`\.

    To learn more about this option, refer to the Spark documentation on [CountVectorizer](https://spark.apache.org/docs/3.0.0/ml-features#countvectorizer)\.
  + **Hashing** is computationally faster\. **Hash vectorize parameters** includes the following:
    + **Number of features during hashing**: A hash vectorizer maps tokens to a vector index according to their hash value\. This feature determines the number of possible hash values\. Large values result in fewer collisions between hash values but a higher dimension output vector\.

    To learn more about this option, refer to the Spark documentation on [FeatureHasher](https://spark.apache.org/docs/3.0.0/ml-features#featurehasher)
+ **Apply IDF**: When chosen, an IDF transformation is applied, which multiplies the term frequency with the standard inverse document frequency used for TF\-IDF embedding\. **IDF parameters** include the following: 
  + **Minimum document frequency **: Minimum number of documents \(rows\) in which a term \(token\) must appear to be included\. If **count\_vectorize** is the chosen vectorizer, we recommend that you keep the default value and only modify the **min\_doc\_freq** field in **Count vectorize parameters**\. Defaults to `5`\.
+ ** Output format**:The output format of each row\. 
  + Select **Vector** to produce a single column with a sparse vector\. 
  + Select **Flattened** to create a column for every category with an indicator variable for whether the text in the original column contains a value that is equal to that category\. You can only choose flattened when **Vectorizer** is set as **Count vectorizer**\.

## Featurize Date/Time<a name="data-wrangler-transform-datetime-embed"></a>

Use **Featurize date/time** to create a vector embedding representing a date/time field\. To use this transform, your date/time data must be in one of the following formats: 
+ Strings describing date/time, for example, `"January 1st, 2020, 12:44pm"`\. 
+ A unix timestamp\. A unix timestamp describes the number of seconds, milliseconds, microseconds, or nanoseconds from 1/1/1970\. 

You can choose to **Infer datetime format** and provide a **Datetime format**\. If you provide a date/time format, you must use the codes described in this [Python documentation](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes)\. The options you select for these two configurations have implications for the speed of the operation, and the final results:
+ The most manual and computationally fastest option is to specify a **Datetime format** and select **No** for **Infer datetime format**\.
+ To reduce manual labor, you can simply choose **Infer datetime format** and not specify a date/time format\. This is also a computationally fast operation; however, the first date/time format encountered in the input column is assumed to be the format for the entire column\. Therefore, if other formats are encountered in the column, these values are NaN in the final output\. Therefore, this option can result in unparsed strings\. 
+ If you do not specify a format and you select **No** for **Infer datetime format**, you get the most robust results\. All valid date/time strings are parsed\. However, this operation can be an order of magnitude slower than the first two options in this list\. 

When you use this transform, you specify an **Input column** which contains date/time data in one of the formats listed above\. The transform creates an output column named **Output column name**\. The format of the output column depends on your configuration using the following:
+ **Vector**: Outputs a single column with a sparse vector\. 
+ **Columns**: Creates a new column for every feature\. For example, if the output contains a year, month, and day, three separate columns are created for year, month, and day\. 

Additionally, you must choose an **Embedding mode**\. For linear models and deep networks, **cyclic** is recommended\. For tree based algorithms, **ordinal** is recommended\.

## Format String<a name="data-wrangler-transform-format-string"></a>

The **Format string** transforms contain standard string formatting operations\. For example, you can use these operations to remove special characters, normalize string lengths, and update string casing\.

This feature group contains the following transforms\. All transforms return copies of the strings in the **Input column** and add the result to a new, output column\.


****  

| Name | Function | 
| --- | --- | 
| Left pad |  Left\-pad the string with a given **Fill character** to the given **width**\. If the string is longer than **width**, the return value is shortened to **width** characters\.  | 
| Right pad |  Right\-pad the string with a given **Fill character** to the given **width**\. If the string is longer than **width**, the return value is shortened to **width** characters\.  | 
| Center \(pad on either side\) |  Center\-pad the string \(add padding on both sides of the string\) with a given **Fill character** to the given **width**\. If the string is longer than **width**, the return value is shortened to **width** characters\.  | 
| Prepend zeros |  Left\-fill a numeric string with zeros, up to a given **width**\. If the string is longer than **width**, the return value is shortened to **width** characters\.  | 
| Strip left and right |  Returns a copy of the string with the leading and trailing characters removed\.  | 
| Strip characters from left |  Returns a copy of the string with leading characters removed\.  | 
| Strip characters from right |  Returns a copy of the string with trailing characters removed\.  | 
| Lower case |  Convert all letters in text to lowercase\.  | 
| Upper case |  Convert all letters in text to uppercase\.  | 
| Capitalize |  Capitalize the first letter in each sentence\.   | 
| Swap case | Converts all uppercase characters to lowercase and all lowercase characters to uppercase characters of the given string, and returns it\. | 
| Add prefix or suffix |  Adds a prefix and a suffix the string column\. You must specify at least one of **Prefix** and **Suffix**\.   | 
| Remove Symbols |  Removes given symbols from a string\. All listed characters will be removed\. Defaults to white space\.   | 

## Handle Outliers<a name="data-wrangler-transform-handle-outlier"></a>

Machine learning models are sensitive to the distribution and range of your feature values\. Outliers, or rare values, can negatively impact model accuracy and lead to longer training times\. Use this feature group to detect and update outliers in your dataset\. 

When you define a **Handle outliers** transform step, the statistics used to detect outliers are generated on the data available in Data Wrangler when defining this step\. These same statistics are used when running a Data Wrangler job\. 

Use the following sections to learn more about the transforms this group contains\. You specify an **Output name** and each of these transforms produces an output column with the resulting data\. 

### Robust standard deviation numeric outliers<a name="data-wrangler-transform-handle-outlier-rstdev"></a>

This transform detects and fixes outliers in numeric features using statistics that are robust to outliers\.

You must define an **Upper quantile** and a **Lower quantile**, which are used in the statistics used to calculate outliers\. You also need to specify the number of **Standard deviations** from which a value must vary from the mean to be considered an outlier\. For example, if you specify 3 for **Standard deviations**, a value must fall more than 3 standard deviations from the mean to be considered an outlier\. 

The **Fix method** is the method used to handle outliers when they are detected\. You can choose from the following:
+ **Clip**: Use this option to clip the outliers to the corresponding outlier detection bound\.
+ **Remove**: Use this to remove rows with outliers from the dataframe\.
+ **Invalidate**: Use this to replace outliers with invalid values\.

### Standard Deviation Numeric Outliers<a name="data-wrangler-transform-handle-outlier-sstdev"></a>

This transform detects and fixes outliers in numeric features using the mean and standard deviation\.

You specify the number of **Standard deviations** a value must vary from the mean to be considered an outlier\. For example, if you specify 3 for **Standard deviations**, a value must fall more than 3 standard deviations from the mean to be considered an outlier\. 

The **Fix method** is the method used to handle outliers when they are detected\. You can choose from the following:
+ **Clip**: Use this option to clip the outliers to the corresponding outlier detection bound\.
+ **Remove**: Use this to remove rows with outliers from the dataframe\.
+ **Invalidate**: Use this to replace outliers with invalid values\.

### Quantile Numeric Outliers<a name="data-wrangler-transform-handle-outlier-quantile-numeric"></a>

Use this transform to detect and fix outliers in numeric features using quantiles\. You can define an **Upper quantile** and a **Lower quantile**, and all values that fall above or below those quantile\-values, respectively, are considered outliers\. 

The **Fix method** is the method used to handle outliers when they are detected\. You can choose from the following:
+ **Clip**: Use this option to clip the outliers to the corresponding outlier detection bound\.
+ **Remove**: Use this to remove rows with outliers from the dataframe\.
+ **Invalidate**: Use this to replace outliers with invalid values\. 

### Min\-Max Numeric Outliers<a name="data-wrangler-transform-handle-outlier-minmax-numeric"></a>

This transform detects and fixes outliers in numeric features using upper and lower thresholds\. Use this method if you know threshold values that demark outliers\.

You specify a **Upper threshold** and a **Lower threshold**, and if values fall above or below those thresholds respectively, they are considered outliers\. 

The **Fix method** is the method used to handle outliers when they are detected\. You can choose from the following:
+ **Clip**: Use this option to clip the outliers to the corresponding outlier detection bound\.
+ **Remove**: Use this to remove rows with outliers from the dataframe\.
+ **Invalidate**: Use this to replace outliers with invalid values\. 

### Replace Rare<a name="data-wrangler-transform-handle-outlier-replace-rare"></a>

When you use the **Replace rare** transform, you specify a threshold and Data Wrangler finds all values that meet that threshold and replaces them with a string that you specify\. For example, you may want to use this transform to categorize all outliers in a column into an "Others" category\. 
+ **Replacement string**: The string with which to replace outliers\.
+ **Absolute threshold**: A category is rare if the number of instances is less than or equal to this absolute threshold\.
+ **Fraction threshold**: A category is rare if the number of instances is less than or equal to this fraction threshold multiplied by the number of rows\.
+ **Max common categories**: Maximum not\-rare categories that remain after the operation\. If the threshold does not filter enough categories, those with the top number of appearances are classified as not rare\. If set to 0 \(default\), there is no hard limit to the number of categories\.

## Handle Missing Values<a name="data-wrangler-transform-handle-missing"></a>

Missing values are a common occurrence in machine learning datasets\. In some situations, it is appropriate to impute missing data with a calculated value, such as an average or categorically common value\. You can process missing values using the **Handle missing values** transform group\. This group contains the following transforms\. 

### Fill Missing<a name="data-wrangler-transform-fill-missing"></a>

Use the **Fill missing** transform to replace missing values with a **Fill value** you define\. 

### Impute Missing<a name="data-wrangler-transform-impute"></a>

Use the **Impute missing** transform to create a new column that contains imputed values where missing values were found in input categorical and numerical data\. The configuration depends on your data type\. Configure this transform using the following:
+ **Imputing strategy**: The strategy used to determine the new value to impute\. 

  For numeric data, the chosen statistic is computed over the present values and is used as the imputed value for all missing values\. The choices are **mean** and **median**\. 

  For categorical data, you can choose to impute the **Most frequent value** in the column, or you can define a custom string to impute\. 

### Add Indicator for Missing<a name="data-wrangler-transform-missing-add-indicator"></a>

Use the **Add indicator for missing** transform to create a new indicator column, which contains a boolean `"false"` if a row contains a value, and `"true"` if a row contains a missing value\. 

### Drop Missing<a name="data-wrangler-transform-drop-missing"></a>

Use the **Drop missing** option to drop rows that contain missing values from the **Input column**\.

## Manage Columns<a name="data-wrangler-manage-columns"></a>

You can use the following transforms to quickly update and manage columns in your dataset: 


****  

| Name | Function | 
| --- | --- | 
| Drop Column | Delete a column\.  | 
| Duplicate Column | Duplicate a column\. | 
| Rename Column | Rename a column\. | 
| Move Column |  Move a column's location in the dataset\. Choose to move your column to the start or end of the dataset, before or after a reference column, or to a specific index\.   | 

## Manage Rows<a name="data-wrangler-transform-manage-rows"></a>

Use this transform group to quickly perform sort and shuffle operations on rows\. This group contains the following:
+ **Sort**: Sort the entire dataframe by a given column\. Select the check box next to **Ascending order** for this option; otherwise, deselect the check box and descending order is used for the sort\. 
+ **Shuffle**: Randomly shuffle all rows in the dataset\. 

## Manage Vectors<a name="data-wrangler-transform-manage-vectors"></a>

Use this transform group to combine or flatten vector columns\. This group contains the following transforms\. 
+ **Assemble**: Use this transform to combine Spark vectors and numeric data into a single column\. For example, you can combine three columns: two containing numeric data and one containing vectors\. Add all the columns you want to combine in **Input columns** and specify a **Output column name** for the combined data\. 
+ **Flatten**: Use this transform to flatten a single column containing vector data\. The input column must contain PySpark vectors, or array\-like objects\. You can control the number of columns that get created by specifying a **Method to detect number of outputs**\. For example, if you select **Length of first vector**, the number of elements in the first valid vector or array found in the column determines the number of output columns that gets created\. All other input vectors with too many items are truncated\. Inputs with too few items are filled with NaNs\.

  You also specify an **Output prefix**, which is used as the prefix for each output column\. 

## Process Numeric<a name="data-wrangler-transform-process-numeric"></a>

Use the **Process Numeric** feature group to process numeric data\. Each scalar in this group is defined using the Spark library\. The following scalars are supported:
+ **Standard Scaler**: Standardize the input column by subtracting the mean from each value and scaling to unit variance\. To learn more, refer to the Spark documentation for [StandardScaler](https://spark.apache.org/docs/3.0.0/ml-features#standardscaler)\.
+ **Robust Scaler**: Scale the input column using statistics that are robust to outliers\. To learn more, refer to the Spark documentation for [RobustScaler](https://spark.apache.org/docs/3.0.0/ml-features#robustscaler)\.
+ **Min Max Scaler**: Transform the input column by scaling each feature to a given range\. To learn more, refer to the Spark documentation for [MinMaxScaler](https://spark.apache.org/docs/3.0.0/ml-features#minmaxscaler)\.
+ **Max Absolute Scaler**: Scale the input column by dividing each value by the maximum absolute value\. To learn more, refer to the Spark documentation for [MaxAbsScaler](https://spark.apache.org/docs/3.0.0/ml-features#maxabsscaler)\.

## Search and Edit<a name="data-wrangler-transform-search-edit"></a>

Use this section to search for and edit specific patterns within strings\. For example, you can find and update strings within sentences or documents, split strings by delimiters, and find occurrences of specific strings\. 

The following transforms are supported under **Search and edit**\. All transforms return copies of the strings in the **Input column** and add the result to a new output column\.


****  

| Name | Function | 
| --- | --- | 
|  Find substring  |  Returns the index of the first occurrence of the **Substring** for which you searched optionally, starting and ending the search at **Start** and **End** respectively\.   | 
|  Find substring \(from right\)  |  Returns the index of the last occurrence of the **Substring** for which you searched, optionally, starting and ending the search at **Start** and **End** respectively\.   | 
|  Matches prefix  |  Returns a Boolean if the string contains a given **Pattern**\. A pattern can be a character sequence or regular expression\. Optionally, you can make the pattern case sensitive\.   | 
|  Find all occurrences  |  Returns an array with all occurrences of a given pattern\. A pattern can be a character sequence or regular expression\.   | 
|  Extract using regex  |  Returns a string that matches a given Regex pattern\.  | 
|  Extract between delimiters  |  Returns a string with all characters found between **Left delimiter** and **Right delimiter**\.   | 
|  Extract from position  |  Returns a string, starting from **Start position** in the input string, that contains all characters up to the start position plus **Length**\.   | 
|  Find and replace substring  |  Returns a string with all matches of a given **Pattern** \(regular expression\) replaced by **Replacement string**\.  | 
|  Replace between delimiters  |  Returns a string with the substring found between the first appearance of a **Left delimiter** and the last appearance of a **Right delimiter** replaced by **Replacement string**\. If no match is found, nothing is replaced\.   | 
|  Replace from position  |  Returns a string with the substring between **Start position** and **Start position** plus **Length** replaced by **Replacement string**\. If **Start position** plus **Length** is greater than the length of the replacement string, the output contains **…**\.  | 
|  Convert regex to missing  |  Converts a string to `None` if invalid and returns the result\. Validity is defined with a regular expression in **Pattern**\.  | 
|  Split string by delimiter  |  Returns an array of strings from the input string, split by **Delimiter**, with up to **Max number of splits** \(optional\)\. Delimiter defaults to white space\.   | 

## Parse Value as Type<a name="data-wrangler-transform-cast-type"></a>

Use this transform to cast a column to a new type\. The supported Data Wrangler data types are:
+ Long
+ Float
+ Boolean
+ Date, in the format dd\-MM\-yyyy, representing day, month, and year respectively\. 
+ String

## Validate String<a name="data-wrangler-transform-validate-string"></a>

Use the **Validate string** transforms to create a new column that indicates that a row of text data meets a specified condition\. For example, you can use a **Validate string** transform to verify that a string only contains lowercase characters\. The following transforms are supported under **Validate string**\. 

The following transforms are included in this transform group\. If a transform outputs a Boolean value, `True` is represented with a `1` and `False` is represented with a `0`\.


****  

| Name | Function | 
| --- | --- | 
|  String length  |  Returns `True` if a string length equals specified length\. Otherwise, returns `False`\.   | 
|  Starts with  |  Returns `True` if a string starts will a specified prefix\. Otherwise, returns `False`\.  | 
|  Ends with  |  Returns `True` if a string length equals specified length\. Otherwise, returns `False`\.  | 
|  Is alphanumeric  |  Returns `True` if a string only contains numbers and letters\. Otherwise, returns `False`\.  | 
|  Is alpha \(letters\)  |  Returns `True` if a string only contains letters\. Otherwise, returns `False`\.  | 
|  Is digit  |  Returns `True` if a string only contains digits\. Otherwise, returns `False`\.  | 
|  Is space  |  Returns `True` if a string only contains numbers and letters\. Otherwise, returns `False`\.  | 
|  Is title  |  Returns `True` if a string contains any white spaces\. Otherwise, returns `False`\.  | 
|  Is lowercase  |  Returns `True` if a string only contains lower case letters\. Otherwise, returns `False`\.  | 
|  Is uppercase  |  Returns `True` if a string only contains upper case letters\. Otherwise, returns `False`\.  | 
|  Is numeric  |  Returns `True` if a string only contains numbers\. Otherwise, returns `False`\.  | 
|  Is decimal  |  Returns `True` if a string only contains decimal numbers\. Otherwise, returns `False`\.  | 