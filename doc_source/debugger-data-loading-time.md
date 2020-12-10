# Profiling Data Loader<a name="debugger-data-loading-time"></a>

In PyTorch, data loader iterators, such as `SingleProcessingDataLoaderIter` and `MultiProcessingDataLoaderIter`, are initiated at the beginning of every iteration over a dataset\. During the initialization phase, PyTorch turns on worker processes depending on the configured number of workers, establishes data queue to fetch data and `pin_memory` threads\.

To use the PyTorch data loader profiling analysis tool, import the following `PT_dataloader_analysis` class:

```
from smdebug.profiler.analysis.utils.pytorch_dataloader_analysis import PT_dataloader_analysis
```

Pass the profiling data retrieved as a Pandas frame data object in the [Access the Profiling Data Using the Pandas Data Parsing Tool](debugger-access-data-profiling-pandas-frame.md) section:

```
pt_analysis = PT_dataloader_analysis(pf)
```

The following functions are available for the `pt_analysis` object:

The SMDebug `S3SystemMetricsReader` class reads the system metrics from the S3 bucket specified to the `s3_trial_path` parameter\.
+ `pt_analysis.analyze_dataloaderIter_initialization()`

  The analysis outputs the median and maximum duration for these initializations\. If there are outliers, \(i\.e duration is greater than 2 \* median\), the function prints the start and end times for those durations\. These can be used to inspect system metrics during those time intervals\.

  The following list shows what analysis is available from this class method:
  + Which type of data loader iterators were initialized\.
  + The number of workers per iterator\.
  + Inspect whether the iterator was initialized with or without pin\_memory\.
  + Number of times the iterators were initialized during training\.
+ `pt_analysis.analyze_dataloaderWorkers()`

  The following list shows what analysis is available from this class method:
  + The number of worker processes that were spun off during the entire training\. 
  + Median and maximum duration for the worker processes\. 
  + Start and end time for the worker processes that are outliers\. 
+ `pt_analysis.analyze_dataloader_getnext()`

  The following list shows what analysis is available from this class method:
  + Number of GetNext calls made during the training\. 
  + Median and maximum duration in microseconds for GetNext calls\. 
  + Start time, End time, duration and worker id for the outlier GetNext call duration\. 
+ `pt_analysis.analyze_batchtime(start_timestamp, end_timestamp, select_events=[".*"], select_dimensions=[".*"])`

  Debugger collects the start and end times of all the GetNext calls\. You can find the amount of time spent by the training script on one batch of data\. Within the specified time window, you can identify the calls that are not directly contributing to the training\. These calls can be from the following operations: computing the accuracy, adding the losses for debugging or logging purposes, and printing the debugging information\. Operations like these can be compute intensive or time consuming\. We can identify such operations by correlating the Python profiler, system metrics, and framework metrics\.

  The following list shows what analysis is available from this class method:
  + Profile time spent on each data batch, `BatchTime_in_seconds`, by finding the difference between start times of current and subsequent GetNext calls\. 
  + Find the outliers in `BatchTime_in_seconds` and start and end time for those outliers\.
  + Obtain the system and framework metrics during those `BatchTime_in_seconds` timestamps\. This indicates where the time was spent\.
+ `pt_analysis.plot_the_window()`

  Plots a timeline charts between a start timestamp and the end timestamp\.