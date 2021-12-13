# Time Series Forecasts in Amazon SageMaker Canvas<a name="canvas-time-series"></a>

Amazon SageMaker Canvas gives you the ability to use machine learning time series forecasts\. Time series forecasts give you the ability to make predictions that can vary with time\.

You can make a time series forecast for the following examples:
+ Forecasting your inventory in the coming months\.
+ The number of items sold in the next four months\.
+ The effect of reducing the price on sales during the holiday season\.
+ Item inventory in the next 12 months\.
+ The number of customers entering a store in the next several hours\.
+ Forecasting how a 10% reduction in the price of a product affects sales over a time period\.

To make a time series forecast, your dataset must have the following:
+ A timestamp column with all values having the `datetime` type using the format: `yyyy-MM-dd HH:mm:ss`\.
+ A target column that has the values that you're using to forecast future values\.

For higher prediction accuracy, your dataset can also have additional columns that can provide data that can explain the variation in the target column\. Using the additional explanatory columns might help you forecast future values in the target column more accurately\.

For example, you can forecast the amount of ice cream sold by a grocery store\. To make a forecast, you must have a timestamp column and a column that indicates how much ice cream the grocery store sold\. For a more accurate forecast, your dataset can also include the price, the ambient temperature, the flavor of the ice cream, or a unique identifier for the ice cream\.

Ice cream sales might increase when the weather is warmer\. A decrease in the price of the ice cream might result in more units sold\. Having a column with ambient temperature data and a column with pricing data can improve your ability to forecast the number of units of ice cream the grocery store sells\.

You might have missing data for different reasons\. The reason for your missing data might inform how you want Amazon SageMaker Canvas to impute it\. For example, your organization might use an automatic system that only tracks when a sale happens\. If you're using a dataset that comes from this type of automatic system, you have missing values in the target column\.

For missing values in the dataset, you can either choose the imputation method that SageMaker Canvas suggests, or choose a different method\.

**Important**  
If you have missing values in the target column, we recommend using a dataset that doesn't have them\. SageMaker Canvas uses the target column to forecast future values\. Missing values in the target column can greatly reduce the accuracy of the forecast\.

You can make one of the following types of forecasts:
+ **Single item**
+ **All items**

For a forecast on all the items in your dataset, SageMaker Canvas returns a forecast for the future values for each item in your dataset\.

For a single item forecast, you specify the item and SageMaker Canvas returns a forecast for the future values\. The forecast includes a line graph that plots the predicted values over time\.

**Topics**
+ [Gain additional insights from your forecast](canvas-additional-insights.md)
+ [Make a time series forecast](canvas-make-time-series-forecast.md)