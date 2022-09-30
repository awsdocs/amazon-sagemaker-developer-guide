# Monitor data quality<a name="model-monitor-data-quality"></a>

Data quality monitoring automatically monitors machine learning \(ML\) models in production and notifies you when data quality issues arise\. ML models in production have to make predictions on real\-life data that is not carefully curated like most training datasets\. If the statistical nature of the data that your model receives while in production drifts away from the nature of the baseline data it was trained on, the model begins to lose accuracy in its predictions\. Amazon SageMaker Model Monitor uses rules to detect data drift and alerts you when it happens\. To monitor data quality, follow these steps:
+ Enable data capture\. This captures inference input and output from a real\-time inference endpoint and stores the data in Amazon S3\. For more information, see [Capture data](model-monitor-data-capture.md)\.
+ Create a baseline\. In this step, you run a baseline job that analyzes an input dataset that you provide\. The baseline computes baseline schema constraints and statistics for each feature using [Deequ](https://github.com/awslabs/deequ), an open source library built on Apache Spark, which is used to measure data quality in large datasets\. For more information, see [Create a Baseline](model-monitor-create-baseline.md)\.
+ Define and schedule data quality monitoring jobs\. For more information, see [Schedule monitoring jobs](model-monitor-scheduling.md)\.
  + Optionally use preprocessing and postprocessing scripts to transform the data coming out of your data quality analysis\. For more information, see [Preprocessing and Postprocessing](model-monitor-pre-and-post-processing.md)\.
+ View data quality metrics\. For more information, see [Schema for Statistics \(statistics\.json file\)](model-monitor-interpreting-statistics.md)\.
+ Integrate data quality monitoring with Amazon CloudWatch\. For more information, see [CloudWatch Metrics](model-monitor-interpreting-cloudwatch.md)\.
+ Interpret the results of a monitoring job\. For more information, see [Interpret results](model-monitor-interpreting-results.md)\.
+ Use SageMaker Studio to enable data quality monitoring and visualize results\. For more information, see [Visualize results in Amazon SageMaker Studio](model-monitor-interpreting-visualize-results.md)\.

**Note**  
Model Monitor computes model metrics and statistics on tabular data only\. For example, an image classification model that takes images as input and outputs a label based on that image can still be monitored\. Model Monitor would be able to calculate metrics and statistics for the output, not the input\.

**Topics**
+ [Create a Baseline](model-monitor-create-baseline.md)
+ [Schema for Statistics \(statistics\.json file\)](model-monitor-interpreting-statistics.md)
+ [CloudWatch Metrics](model-monitor-interpreting-cloudwatch.md)
+ [Schema for Violations \(constraint\_violations\.json file\)](model-monitor-interpreting-violations.md)