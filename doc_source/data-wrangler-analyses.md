# Analyze and Visualize<a name="data-wrangler-analyses"></a>

Amazon SageMaker Data Wrangler includes built\-in analyses that help you generate visualizations and data analyses in a few clicks\. You can also create custom analyses using your own code\. 

You add an analysis to a dataframe by selecting a step in your data flow, and then choosing **Add analysis**\. To access an analysis you've created, select the step that contains the analysis, and select the analysis\. 

All analyses are generated using 100,000 rows of your dataset\. 

You can add the following analysis to a dataframe:
+ Data visualizations, including histograms and scatter plots\. 
+ A quick summary of your dataset, including number of entries, minimum and maximum values \(for numeric data\), and most and least frequent categories \(for categorical data\)\.
+ A quick model of the dataset, which can be used to generate an importance score for each feature\. 
+ A target leakage report, which you can use to determine if one or more features are strongly correlated with your target feature\.
+ A custom visualization using your own code\. 

Use the following sections to learn more about these options\. 

## Histogram<a name="data-wrangler-visualize-histogram"></a>

Use histograms to see the counts of feature values for a specific feature\. You can inspect the relationships between features using the **Color by** option\. For example, the following histogram charts the distribution of user ratings of the best\-selling books on Amazon from 2009–2019, colored by genre\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/histogram.png)

You can use the **Facet by** feature to create histograms of one column, for each value in another column\. For example, the following diagram shows histograms of user reviews of best\-selling books on Amazon if faceted by year\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/review_by_year.png)

## Scatter Plot<a name="data-wrangler-visualize-scatter-plot"></a>

Use the **Scatter Plot** feature to inspect the relationship between features\. To create a scatter plot, select a feature to plot on the **X axis** and the **Y axis**\. Both of these columns must be numeric typed columns\. 

You can color scatter plots by an additional column\. For example, the following example shows a scatter plot comparing the number of reviews against user ratings of top\-selling books on Amazon between 2009 and 2019\. The scatter plot is colored by book genre\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/scatter-plot.png)

Additionally, you can facet scatter plots by features\. For example, the following shows an example of the same review versus user rating scatter plot, faceted by year\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/scatter-plot-facet.png)

## Table Summary<a name="data-wrangler-table-summary"></a>

Use the **Table Summary** analysis to quickly summarize your data\.

For columns with numerical data, including log and float data, a table summary reports the number of entries \(count\), minimum \(min\), maximum \(max\), mean, and standard deviation \(stddev\) for each column\.

For columns with non\-numerical data, including columns with string, Boolean, or date/time data, a table summary reports the number of entries \(count\), least frequent value \(min\), and most frequent value \(max\)\. 

## Quick Model<a name="data-wrangler-quick-model"></a>

Use the **Quick Model** visualization to quickly evaluate your data and produce importance scores for each feature\. A [feature importance score](http://spark.apache.org/docs/2.1.0/api/python/pyspark.ml.html#pyspark.ml.classification.DecisionTreeClassificationModel.featureImportances) score indicates how useful a feature is at predicting a target label\. The feature importance score is between \[0, 1\] and a higher number indicates that the feature is more important to the whole dataset\. On the top of the quick model chart, there is a model score\. A classification problem shows an F1 score\. A regression problem has a mean squared error \(MSE\) score\.

When you create a quick model chart, you select a dataset you want evaluated, and a target label against which you want feature importance to be compared\. Data Wrangler does the following:
+ Infers the data types for the target label and each feature in the dataset selected\. 
+ Determines the problem type\. Based on the number of distinct values in the label column, Data Wrangler determines if this is a regression or classification problem type\. Data Wrangler sets a categorical threshold to 100\. If there are more than 100 distinct values in the label column, Data Wrangler classifies it as a regression problem; otherwise, it is classified as a classification problem\. 
+ Pre\-process features and label data for training\. The algorithm used requires encoding features to vector type and encoding labels to double type\. 
+ Trains a random forest algorithm with 70% of data\. Spark’s [RandomForestRegressor](https://spark.apache.org/docs/latest/ml-classification-regression.html#random-forest-regression) is used to train a model for regression problems\. The [RandomForestClassifier](https://spark.apache.org/docs/latest/ml-classification-regression.html#random-forest-classifier) is used to train a model for classification problems\.
+ Evaluates a random forest model with the remaining 30% of data\. Data Wrangler evaluates classification models using an F1 score and evaluates regression models using an MSE score\.
+ Calculates feature importance for each feature using the Gini importance method\. 

Below is the user interface for the Quick Model feature\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/quick-model.png)

## Target Leakage<a name="data-wrangler-analysis-target-leakage"></a>

Target leakage occurs when there is data in a machine learning training dataset that is strongly correlated with the target label, but is not available in real\-world data\. For example, you may have a column in your dataset that serves as a proxy for the column you want to predict with your model\. 

When you use the **Target Leakage** analysis, you specify the following:
+ **Target**: This is the feature about which you want your ML model to be able to make predictions\.
+ **Problem type**: This is the ML problem type on which you are working\. Problem type can either be **classification** or **regression**\. 
+  \(Optional\) **Max features**: This is the maximum number of features to present in the visualization, which shows features ranked by their risk of being target leakage\.

For classification, the Target Leakage analysis uses the area under the receiver operating characteristic, or AUC \- ROC curve for each column, up to **Max features**\. For regression, it uses a coefficient of determination, or R2 metric\.

The AUC \- ROC curve provides a predictive metric, computed individually for each column using cross\-validation, on a sample of up to around 1000 rows\. A score of 1 indicates perfect predictive abilities, which often indicates target leakage\. A score of 0\.5 or lower indicates that the information on the column could not provide, on its own, any useful information towards predicting the target\. Although it can happen that a column is uninformative on its own but is useful in predicting the target when used in tandem with other features, a low score could indicate the feature is redundant\.

For example, the following image shows a target leakage report for a diabetes classification problem, that is, predicting if a person has diabetes or not\. An AUC \- ROC curve is used to calculate the predictive ability of five features, and all are determined to be safe from target leakage\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/target-leakage.png)

## Bias Report<a name="data-wrangler-bias-report"></a>

You can use the bias report in Data Wrangler to uncover potential biases in your data\. To generate a bias report, you must specify the target column, or **Label**, that you want to predict and a **Facet**, or the column that you want to inspect for biases\.

**Label** – The feature about which you want a model to make predictions\. For example, if you are predicting customer conversion, you may select a column containing data on whether or not a customer has placed an order\. You must also specify whether this feature is a label or a threshold\. If you specify a label, you must specify what a *positive outcome* looks like in your data\. In the customer conversion example, a positive outcome may be a 1 in the orders column, representing the positive outcome of a customer placing an order within the last three months\. If you specify a threshold, you must specify a lower bound defining a positive outcome\. For example, if your customer orders columns contains the number of orders placed in the last year, you may want to specify 1\.

**Facet** – The column that you want to inspect for biases\. For example, if you are trying to predict customer conversion, your facet may be the age of the customer\. You may choose this facet because you believe that your data is biased toward a certain age group\. You must identify whether the facet is measured as a value or threshold\. For example, if you wanted to inspect one or more specific ages, you select **Value** and specify those ages\. If you want to look at an age group, you select **Threshold** and specify the threshold of ages you want to inspect\.

After you select your feature and label, you select the types of bias metrics you want to calculate\.

To learn more, see [Generate reports for bias in pre\-training data](https://docs.aws.amazon.com/sagemaker/latest/dg/data-bias-reports.html)\. 

## Create Custom Visualizations<a name="data-wrangler-visualize-custom"></a>

Use the **Code** tab to create custom visualizations\. Your dataset, after undergoing the most recently added transformation, is available as a [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html) in this code block via the `df` variable\. 

You must provide an output variable, `chart`, to store an [Altair](https://altair-viz.github.io/) output chart\. For example, the following is an example of a code block that can be used to generate a custom histogram using the titanic dataset\.

```
import altair as alt
df = df.iloc[:30]
df = df.rename(columns={"Age": "value"})
df = df.assign(count=df.groupby('value').value.transform('count'))
df = df[["value", "count"]]
base = alt.Chart(df)
bar = base.mark_bar().encode(x=alt.X('value', bin=True, axis=None), y=alt.Y('count'))
rule = base.mark_rule(color='red').encode(
    x='mean(value):Q',
    size=alt.value(5))
chart = bar + rule
```

**To create a custom visualization:**

1. In the **Visualize** area of Data Wrangler, choose the **Code** tab\.

1. Give your visualization a **Name**\.

1. Enter your code in the code box\. 

1. Choose **Preview** to preview your visualization\.

1. Choose **Add Visualization** to add your visualization\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/visualize-custom.png)