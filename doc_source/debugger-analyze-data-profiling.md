# Access the Monitoring and Profiling Data<a name="debugger-analyze-data-profiling"></a>

The SMDebug `TrainingJob` class reads data from the S3 bucket where the system and framework metrics are saved\. 

**To set up a `TrainingJob` object and retrieve profiling event files of a training job**

```
from smdebug.profiler.analysis.notebook_utils.training_job import TrainingJob
tj = TrainingJob(training_job_name, region)
```

**Tip**  
You need to specify the `training_job_name` and `region` parameters to log to a training job\. There are two ways to specify the training job information:   
Use the SageMaker Python SDK while the estimator is still attached to the training job\.  

  ```
  import sagemaker
      training_job_name=estimator.latest_training_job.job_name
      region=sagemaker.Session().boto_region_name
  ```
Pass strings directly\.  

  ```
  training_job_name="your-training-job-name-YYYY-MM-DD-HH-MM-SS-SSS"
      region="us-west-2"
  ```

**To retrieve a description of the training job description and the S3 bucket URI where the metric data are saved**

```
tj.describe_training_job()
tj.get_config_and_profiler_s3_output_path()
```

**To check if the system and framework metrics are available from the S3 URI**

```
tj.wait_for_sys_profiling_data_to_be_available()
tj.wait_for_framework_profiling_data_to_be_available()
```

**To create system and framework reader objects after the metric data become available**

```
system_metrics_reader = tj.get_systems_metrics_reader()
framework_metrics_reader = tj.get_framework_metrics_reader()
```

**To refresh and retrieve the latest training event files**

The reader objects have an extended method, `refresh_event_file_list()`, to retrieve the latest training event files\.

```
system_metrics_reader.refresh_event_file_list()
framework_metrics_reader.refresh_event_file_list()
```