# Plot the System Metrics and Framework Metrics Data<a name="debugger-access-data-profiling-default-plot"></a>

You can use the system and algorithm metrics objects for the following visualization classes to plot timeline graphs and histograms\.

**Note**  
To visualize the data with narrowed\-down metrics in the following visualization object plot methods, specify `select_dimensions` and `select_events` parameters\. For example, if you specify `select_dimensions=["GPU"]`, the plot methods filter the metrics that include the "GPU" keyword\. If you specify `select_events=["total"]`, the plot methods filter the metrics that include the "total" event tags at the end of the metric names\. If you enable these parameters and give the keyword strings, the visualization classes return the charts with filtered metrics\.
+ The `MetricsHistogram` class

  ```
  from smdebug.profiler.analysis.notebook_utils.metrics_histogram import MetricsHistogram
  
  metrics_histogram = MetricsHistogram(system_metrics_reader)
  metrics_histogram.plot(
      starttime=0, 
      endtime=system_metrics_reader.get_timestamp_of_latest_available_file(), 
      select_dimensions=["CPU", "GPU", "I/O"], # optional
      select_events=["total"]                  # optional
  )
  ```
+ The `StepTimelineChart` class

  ```
  from smdebug.profiler.analysis.notebook_utils.step_timeline_chart import StepTimelineChart
  
  view_step_timeline_chart = StepTimelineChart(framework_metrics_reader)
  ```
+ The `StepHistogram` class

  ```
  from smdebug.profiler.analysis.notebook_utils.step_histogram import StepHistogram
  
  step_histogram = StepHistogram(framework_metrics_reader)
  step_histogram.plot(
      starttime=step_histogram.last_timestamp - 5 * 1000 * 1000, 
      endtime=step_histogram.last_timestamp, 
      show_workers=True
  )
  ```
+ The `TimelineCharts` class

  ```
  from smdebug.profiler.analysis.notebook_utils.timeline_charts import TimelineCharts
  
  view_timeline_charts = TimelineCharts(
      system_metrics_reader, 
      framework_metrics_reader,
      select_dimensions=["CPU", "GPU", "I/O"], # optional
      select_events=["total"]                  # optional 
  )
  
  view_timeline_charts.plot_detailed_profiler_data([700,710])
  ```
+ The `Heatmap` class

  ```
  from smdebug.profiler.analysis.notebook_utils.heatmap import Heatmap
  
  view_heatmap = Heatmap(
      system_metrics_reader,
      framework_metrics_reader,
      select_dimensions=["CPU", "GPU", "I/O"], # optional
      select_events=["total"],                 # optional
      plot_height=450
  )
  ```