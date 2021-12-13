# Make a time series forecast<a name="canvas-make-time-series-forecast"></a>

To make a time series forecast, you choose a target column\. The target column contains the data that you want to predict\. For example, your target column might have data on the number of items sold\. After you select the target column, Amazon SageMaker Canvas selects a **Model type**\. SageMaker Canvas uses the time\-series data to automatically choose a time series model that you can use to make predictions on your data\. After you build the model, you can evaluate its performance and use it to make predictions on new data\.

Use the following procedure to make a time series forecast\.

To make a time\-series forecast, do the following\.

1. Import the data\.

1. Choose a target column in your dataset\.

1. SageMaker Canvas automatically chooses **Time series forecasting** as the model type\. Choose **Set configuration** to confirm that you're performing a time series forecast\.

1. Specify the following fields:
   + **Item ID column** – The column that contains unique identifiers for each item in your dataset\. For example, an SKU number uniquely identifies an item\.
   + Optional: **Group column** – Groups the time series forecast by values in the column\. For example, you can group your forecast for an item by store\.
   + **Time stamp column** – The column containing the time stamps in your dataset\.
   + **Future timestamp** – A timestamp that indicates a future forecast time\. SageMaker Canvas forecasts values up to the point in time specified by the timestamp\.
   + Optional: **Holiday schedule** – Activate the holiday schedule to use a country's holiday schedule\. Use it to make your forecasts with holiday data more accurate\.

You can have one of the following types of missing values:
+ Missing future values
+ Missing values

Missing future values are missing values in the target column\. SageMaker Canvas uses the values in the target column to forecast the values in the future\. If you have missing values in the target column, your forecast might be less accurate\. We highly recommend updating the dataset\.

Missing values are values that are missing in any column other than the target column\. With missing values that aren't in the target column, it's helpful to note the following:
+ They generally don't reduce the accuracy of your forecast as much as missing future values\.
+ SageMaker Canvas automatically imputes the missing values\. You can also specify the imputation method yourself\.

You can evaluate the model by seeing how close the predictions are within the actual value\. You can also use the **Column Impact** metric to determine the direction and magnitude of the column's impact on the model's predictions\. For example, in the following image, holidays had the largest positive impact on the forecast for demand\. Price had the largest negative impact on demand\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-forecast/predictions-zero-state.png)

After you've built a model, you can make the following types of forecasts:
+ Single item – Make a forecast for a single item in a dataset and a line graph of the values that SageMaker Canvas forecasts\. For example, you can see how sales of an item vary over time\.
+ All items – Make a forecast for all items in a dataset\.
+ What\-if scenario – See how changing values in the dataset can affect the overall forecast for a single item\.

The following image shows a single item forecast with a what\-if scenario\. In a what\-if scenario, you have the ability to change values that can vary with time\. You can see how changing the values affects the forecast\.

The points connected by the solid blue line are the values that the model forecasts\. The points connected by the dashed lines show the what\-if scenario\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-forecast/canvas-what-if.png)