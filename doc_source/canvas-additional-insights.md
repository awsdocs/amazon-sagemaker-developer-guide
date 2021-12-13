# Gain additional insights from your forecast<a name="canvas-additional-insights"></a>

In Amazon SageMaker Canvas, you can use the following optional methods to get more insights from your forecast:
+ Group column
+ Holiday schedule
+ What\-if scenario

You can specify a column in your dataset as a **Group column**\. Amazon SageMaker Canvas groups the forecast by each value in the column\. For example, you can group the forecast on columns containing price data or unique item identifiers\. Grouping a forecast by a column lets you make more specific forecasts\. For example, if you group a forecast on a column containing item identifiers, you can see the forecast for each item\.

Overall sales of items might be impacted by the presence of holidays\. For example, in the United States, the number of items sold in both November and December might differ greatly from the number of items sold in January\. If you use the data from November and December to forecast the sales in January, your results might be inaccurate\. Using a holiday schedule prevents you getting inaccurate results\. You can use a holiday schedule for 66 countries\.

For a forecast on a single item in your dataset, you can use what\-if scenarios\. A what\-if scenario gives you the ability to change values in your data and change the forecast\. For example, you can answer the following questions by using a what\-if scenario, "What if I lowered prices? How would that affect the number of items sold?"