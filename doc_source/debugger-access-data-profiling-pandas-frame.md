# Access the Profiling Data Using the Pandas Data Parsing Tool<a name="debugger-access-data-profiling-pandas-frame"></a>

The following `PandasFrame` class provides tools to convert the collected profiling data to Pandas data frame\. 

```
from smdebug.profiler.analysis.utils.profiler_data_to_pandas import PandasFrame
```

The `PandasFrame` class takes the `tj` object's S3 bucket output path, and its methods `get_all_system_metrics()` `get_all_framework_metrics()` return system metrics and framework metrics in the Pandas data format\.

```
pf = PandasFrame(tj.profiler_s3_output_path)
system_metrics_df = pf.get_all_system_metrics()
framework_metrics_df = pf.get_all_framework_metrics(
    selected_framework_metrics=[
        'Step:ModeKeys.TRAIN', 
        'Step:ModeKeys.GLOBAL'
    ]
)
```