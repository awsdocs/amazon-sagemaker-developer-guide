# Merge Timelines of Different Profiling Trace Files<a name="debugger-merge-timeline"></a>

The SMDebug client library provide profiling analysis and visualization tools for merging timelines of system metrics, framework metrics, and Python profiling data collected by Debugger\. 

**Tip**  
Before proceeding, you need to set a TrainingJob object that will be utilized throughout the examples in this page\. For more information about setting up a TrainingJob object, see [Access the Monitoring and Profiling Data](debugger-analyze-data-profiling.md)\.

The `MergedTimeline` class provides tools to integrate and correlate different profiling information in a single timeline\. After Debugger captures profiling data and annotations from different phases of a training job, JSON files of trace events are saved in a default `tracefolder` directory\.
+ For annotations in the Python layers, the trace files are saved in `*pythontimeline.json`\. 
+ For annotations in the TensorFlow C\+\+ layers, the trace files are saved in `*model_timeline.json`\. 
+ Tensorflow profiler saves events in a `*trace.json.gz` file\. 

**Tip**  
If you want to list all of the JSON trace files, use the following AWS CLI command:  

```
! aws s3 ls {tj.profiler_s3_output_path} --recursive | grep '\.json$'
```

As shown in the following animated screenshot, putting and aligning the trace events captured from the different profiling sources in a single plot can provide an overview of the entire events occurring in different phases of the training job\.

![\[An example of merged timeline\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-merged-timeline.gif)

**Tip**  
To interact with the merged timeline on the traicing app using a keyboard, use the `W` key for zooming in, the `A` key for shifting to the left, the `S` key for zooming out, and the `D` key for shifiting to the right\.

The multiple event trace JSON files can be merged into one trace event JSON file using the following `MergedTimeline` API operation and class method from the `smdebug.profiler.analysis.utils.merge_timelines` module\.

```
from smdebug.profiler.analysis.utils.merge_timelines import MergedTimeline

combined_timeline = MergedTimeline(path, file_suffix_filter, output_directory)
combined_timeline.merge_timeline(start, end, unit)
```

The `MergedTimeline` API operation passes the following parameters:
+ `path` \(str\) – Specify a root folder \(`/profiler-output`\) that contains system and framework profiling trace files\. You can locate the `profiler-output` using the SageMaker estimator classmethod or the TrainingJob object\. For example, `estimator.latest_job_profiler_artifacts_path()` or `tj.profiler_s3_output_path`\.
+ `file_suffix_filter` \(list\) – Specify a list of file suffix filters to merge timelines\. Available suffiex filters are `["model_timeline.json", "pythontimeline.json", "trace.json.gz"].` If this parameter is not manually specified, all of the trace files are merged by default\.
+ `output_directory` \(str\) – Specify a path to save the merged timeline JSON file\. The default is to the directory specified for the `path` parameter\.

The `merge_timeline()` classmethod passes the following parameters to execute the merging process:
+ `start` \(int\) – Specify start time \(in microseconds and in Unix time format\) or start step to merge timelines\.
+ `end` \(int\) – Specify end time \(in microseconds and in Unix time format\) or end step to merge timelines\.
+ `unit` \(str\) – Choose between `"time"` and `"step"`\. The default is `"time"`\.

Using the following example codes, execute the `merge_timeline()` method and download the merged JSON file\. 
+ Merge timeline with the `"time"` unit option\. The following example code merges all available trace files between the Unix start time \(the absolute zero Unix time\) and the current Unix time, which means that you can merge the timelines for the entire training duration\.

  ```
  import time
  from smdebug.profiler.analysis.utils.merge_timelines import MergedTimeline
  from smdebug.profiler.profiler_constants import CONVERT_TO_MICROSECS
  
  combined_timeline = MergedTimeline(tj.profiler_s3_output_path, output_directory="./")
  combined_timeline.merge_timeline(0, int(time.time() * CONVERT_TO_MICROSECS))
  ```
+ Merge timeline with the `"step"` unit option\. The following example code merges all available timelines between step 3 and step 9\.

  ```
  from smdebug.profiler.analysis.utils.merge_timelines import MergedTimeline
  
  combined_timeline = MergedTimeline(tj.profiler_s3_output_path, output_directory="./")
  combined_timeline.merge_timeline(3, 9, unit="step")
  ```

Open the Chrome tracing app at [chrome://tracing](chrome://tracing) on a Chrome browser, and open the JSON file\. You can explore the output to plot the merged timeline\. 