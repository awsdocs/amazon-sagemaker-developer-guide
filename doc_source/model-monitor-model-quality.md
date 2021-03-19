# Monitor Model Quality<a name="model-monitor-model-quality"></a>

Model quality monitoring jobs monitor the performance of a model by comparing the predictions that the model makes with the actual ground truth labels that the model attempts to predict\. To do this, model quality monitoring merges data that is captured from real\-time inference with actual labels that you store in an Amazon S3 bucket, and the compares the predictions with the actual labels\.

To measure model quality, model monitor uses metrics that depend on the ML problem type\. For example, if your model is for a regression problem, one of the metrics evaluated is mean square error \(mse\)\. For information about all of the metrics used for the different ML problem types, see [Model Quality Metrics](model-monitor-model-quality-metrics.md)\. 

Model quality monitoring follows the same steps as data quality monitoring, but adds the additional step of merging the actual labels from Amazon S3 with the predictions captured from the real\-time inference endpoint\. To monitor model quality, follow these steps:
+ Enable data capture\. This captures inference input and output from a real\-time inference endpoint and stores the data in Amazon S3\. For more information, see [Capture Data](model-monitor-data-capture.md)\.
+ Create a baseline\. In this step, you run a baseline job that compares predictions from the model with ground truth labels in a baseline dataset\. The baseline job automatically creates baseline statistical rules and constraints that define thresholds against which the model performance is evaluated\. For more information, see [Create a Model Quality Baseline](model-monitor-model-quality-baseline.md)\.
+ Define and schedule model quality monitoring jobs\. For more information, see [Schedule Model Quality Monitoring Jobs](model-monitor-model-quality-schedule.md)\.
+ Ingest ground truth labels that model monitor merges with captured prediction data from real\-time inference endpoint\. For more information, see [Ingest Ground Truth Labels and Merge Them With Predictions](model-monitor-model-quality-merge.md)\.
+ Integrate model quality monitoring with Amazon CloudWatch\. For more information, see [Model Quality CloudWatch Metrics](model-monitor-model-quality-cw.md)\.
+ Interpret the results of a monitoring job\. For more information, see [Interpret Results](model-monitor-interpreting-results.md)\.
+ Use SageMaker Studio to enable model quality monitoring and visualize results\. For more information, see [Visualize Results in Amazon SageMaker Studio](model-monitor-interpreting-visualize-results.md)\.

**Topics**
+ [Create a Model Quality Baseline](model-monitor-model-quality-baseline.md)
+ [Schedule Model Quality Monitoring Jobs](model-monitor-model-quality-schedule.md)
+ [Ingest Ground Truth Labels and Merge Them With Predictions](model-monitor-model-quality-merge.md)
+ [Model Quality Metrics](model-monitor-model-quality-metrics.md)
+ [Model Quality CloudWatch Metrics](model-monitor-model-quality-cw.md)