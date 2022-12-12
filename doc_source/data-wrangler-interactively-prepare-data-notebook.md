# Use an Interactive Data Preparation Widget in an Amazon SageMaker Studio Notebook to Get Data Insights<a name="data-wrangler-interactively-prepare-data-notebook"></a>

Use the Data Wrangler data preparation widget to interact with your data, get visualizations, explore actionable insights, and fix data quality issues\. 

You can access the data preparation widget from an Amazon SageMaker Studio notebook\. For each column, the widget creates a visualization that helps you better understand its distribution\. If a column has data quality issues, a warning appears in its header\.

To see the data quality issues, select the column header showing the warning\. You can use the information that you get from the insights and the visualizations to apply the widget's built\-in transformations to help you fix the issues\. 

For example, the widget might detect that you have a column that only has one unique value and show you a warning\. The warning provides the option to drop the column from the dataset\.

## Getting started with running the widget<a name="data-wrangler-interactively-prepare-data-notebook-getting-started"></a>

Use the following information to help you get started with running a notebook\.

Open a notebook in Amazon SageMaker Studio\. For information about opening a notebook, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\.

**Important**  
To run the widget, the notebook must use one of the following images:  
Python 3 \(Data Science\) with Python 3\.7
Python 3 \(Data Science 2\.0\) with Python 3\.8
Python 3 \(Data Science 3\.0\) with Python 3\.10
SparkAnalytics 1\.0
SparkAnalytics 2\.0
For more information about images, see [Available Amazon SageMaker Images](notebooks-available-images.md)\.

Use the following code to import the data preparation widget and pandas\. The widget uses pandas dataframes to analyze your data\.

```
import pandas as pd
import sagemaker_datawrangler
```

The following example code loads a file into the dataframe called `df`\.

```
df = pd.read_csv("example-dataset.csv")
```

You can use a dataset in any format that you can load as a pandas dataframe object\. For more information about pandas formats, see [IO tools \(text, CSV, HDF5, …\)](https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html)\.

The following cell runs the `df` variable to start the widget\.

```
df
```

The top of the dataframe has the following options:
+ **View the Pandas table** – Switches between the interactive visualization and a pandas table\.
+ **Use all of the rows in your dataset to compute the insights\. Using the entire dataset might increase the time it takes to generate the insights\.** – If you don't select the option, Data Wrangler computes the insights for the first 10,000 rows of the dataset\.

The dataframe shows the first 1000 rows of the dataset\. Each column header has a stacked bar chart that shows the column's characteristics\. It shows the proportion of valid values, invalid values, and missing values\. You can hover over the different portions of the stacked bar chart to get the calculated percentages\.

Each column has a visualization in the header\. The following shows the types of visualizations the columns can have:
+ Categorical – Bar chart
+ Numeric – Histogram
+ Datetime – Bar chart
+ Text – Bar chart

For each visualization, the data preparation widget highlights outliers in orange\.

When you choose a column, it opens a side panel\. The side panel shows you the **Insights** tab\. The pane provides a count for the following types of values:
+ Invalid values – Values whose type doesn’t match the column type\.
+ Missing values – Values that are missing, such as `NaN` or `None`\.
+ Valid values – Values that are neither missing nor invalid\.

For numeric columns, the **Insights** tab shows the following summary statistics:
+ Minimum – The smallest value\.
+ Maximum – The largest value\.
+ Mean – The mean of the values\.
+ Mode – The value that appears most frequently\.
+ Standard deviation – The standard deviation of the values\.

For categorical columns, the **Insights** tab shows the following summary statistics:
+ Unique values – The number of unique values in the column\.
+ Top – The value that appears most frequently\.

The columns that have warning icons in their headers have data quality issues\. Choosing a column opens a **Data quality** tab that you can use to find transforms to help you fix the issue\. A warning has one of the following severity levels:
+ Low – Issues that might not affect your analysis, but can be useful to fix\.
+ Medium – Issues that are likely to affect your analysis, but are likely not critical to fix\.
+ High – Severe issues that we strongly recommend fixing\.

**Note**  
The widget sorts the column to show the values that have data quality issues at the top of the dataframe\. It also highlights the values that are causing the issues\. The color of the highlighting corresponds to the severity level\.

Under **SUGGESTED TRANSFORMS**, you can choose a transform to fix the data quality issue\. The widget can offer multiple transforms that can fix the issue\. It can offer recommendations for the transforms that are best suited to the problem\. You can move your cursor over the transform to get more information about it\.

To apply a transform to the dataset, choose **Apply and export code**\. The transform modifies the dataset and updates the visualization with modified values\. The code for the transform appears in the following cell of the notebook\. If you apply additional transforms to the dataset, the widget appends the transforms to the cell\. You can use the code that the widget generates to do the following:
+ Customize it to better fit your needs\.
+ Use it in your own workflows\.

You can reproduce all the transforms you've made by rerunning all of the cells in the notebook\.

The widget can provide insights and warnings for the target column\. The target column is the column that you're trying to predict\. Use the following procedure to get target column insights\.

To get target column insights, do the following\.

1. Choose the column that you're using as the target column\.

1. Choose **Select as target column**\.

1. Choose the problem type\. The widget's insights and warnings are tailored to the problem types\. The following are the problem types:
   + **Classification** – The target column has categorical data\.
   + **Regression** – The target column has numeric data\.

1. Choose **Run**\.

1. \(Optional\) Under **Target Column Insights**, choose one of the suggested transforms\.

## Reference for the insights and transforms in the widget<a name="data-wrangler-notebook-dataprep-assistant-reference"></a>

For feature columns \(columns that aren't the target column\), you can get the following insights to warn you about issues with your dataset\.
+ **Missing values** – The column has missing values such as `None`, `NaN` \(not a number\), or `NaT` \(not a timestamp\)\. Many machine learning algorithms don’t support missing values in the input data\. Filling them in or dropping the rows with missing data is therefore a crucial data preparation step\. If you see the missing values warning, you can use one of the following transforms to correct the issue\.
  + **Drop missing** – Drops rows with missing values\. We recommend dropping rows when the percentage of rows with missing data is small and imputing the missing values isn't appropriate\. 
  + **Replace with new value** – Replaces textual missing values with `Other`\. You can change `Other` to a different value in the output code\. Replaces numeric missing values with 0\.
  + **Replace with mean** – Replaces missing values with the mean of the column\.
  + **Replace with median** – Replaces missing values with the median of the column\.
  + **Drop column** – Drops the column with missing values from the dataset\. We recommend dropping the entire column when there's a high percentage of rows with missing data\.
+ **Disguised missing values** – The column has disguised missing values\. A disguised missing value is a value that isn't explicitly encoded as a missing value\. For example, instead of using a `NaN` to indicate a missing value, the value could be `Placeholder`\. You can use one of the following transforms to handle the missing values:
  + **Drop missing** – Drops rows with missing values
  + **Replace with new value** – Replaces textual missing values with `Other`\. You can change `Other` to a different value in the output code\. Replaces numeric missing values with 0\.
+ **Constant column** – The column only has one value\. It therefore has no predictive power\. We strongly recommend using the **Drop column** transform to drop the column from the dataset\.
+ **ID column** – The column has no repeating values\. All of the values in the column are unique\. They might be either IDs or database keys\. Without additional information, the column has no predictive power\. We strongly recommend using the **Drop column** transform to drop the column from the dataset\.
+ **High cardinality** – The column has a high percentage of unique values\. High cardinality limits the predictive power of categorical columns\. Examine the importance of the column in your analysis and consider using the **Drop column** transform to drop it\.

For the target column, you can get the following insights to warn you about issues with your dataset\. You can use the suggested transformation provided with the warning to correct the issue\.
+ **Mixed data types in target \(Regression\)** – There are some non\-numeric values in the target column\. There might be data entry errors\. We recommend removing the rows that have the values that can't be converted\.
+ **Frequent label** – Certain values in the target column appear more frequently than what would be normal in the context of regression\. There might be an error in data collection or processing\. A frequently appearing category might indicate that either the value is used as a default value or that it’s a placeholder for missing values\. We recommend using the **Replace with new value** transform to replace the missing values with `Other`\.
+ **Too few instances per class** – The target column has categories that appear rarely\. Some of the categories don't have enough rows for the target column to be useful\. You can use one of the following transforms:
  + **Drop rare target** – Drops unique values with fewer than ten observations\. For example, drops the value `cat` if it appears nine times in the column\.
  + **Replace rare target** – Replaces categories that appear rarely in the dataset with the value `Other`\.
+ **Classes too imbalanced \(multi\-class classification\)** – There are categories in the dataset that appear much more frequently than the other categories\. The class imbalance might affect prediction accuracy\. For the most accurate predictions possible, we recommend updating the dataset with rows that have the categories that currently appear less frequently\.
+ **Large amount of classes/too many classes** – There's a large number of classes in the target column\. Having many classes might result in longer training times or poor predictive quality\. We recommend doing one of the following:
  + Grouping some of the categories into their own category\. For example, if six categories are closely related, we recommend using a single category for them\.
  + Using an ML algorithm that's resilient to multiple categories\.