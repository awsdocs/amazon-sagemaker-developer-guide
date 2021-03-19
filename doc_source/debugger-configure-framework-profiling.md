# Configure Debugger Framework Profiling<a name="debugger-configure-framework-profiling"></a>

To enable Debugger framework profiling, configure the `framework_profile_params` parameter when you construct an estimator\. Debugger framework profiling collects framework metrics, such as data from initialization stage, data loader processes, Python operators of deep learning frameworks and training scripts, detailed profiling within and between steps, with cProfile or Pyinstrument options\. Using the `FrameworkProfile` class, you can configure custom framework profiling options\. 

**Note**  
Before getting started with Debugger framework profiling, verify that the framework used to build your model is supported by Debugger for framework profiling\. For more information, see [Supported Frameworks and Algorithms](train-debugger.md#debugger-supported-frameworks)\.   
Debugger saves the framework metrics in a default S3 bucket\. The format of the default S3 bucket URI is `s3://sagemaker-<region>-<12digit_account_id>/<training-job-name>/profiler-output/`\.

## Start a Training Job with the Default System Monitoring and Framework Profiling<a name="debugger-configure-framework-profiling-basic"></a>

The following example code is the simplest `profiler_config` parameter setting to start the default system monitoring and the default framework profiling\. The `FrameworkProfile` class in the following example code initiates the default framework profiling when a training job starts\. Debugger framework profiling includes the following options: detailed profiling, data loader profiling, and Python profiling\.

```
from sagemaker.debugger import ProfilerConfig, FrameworkProfile
    
profiler_config=ProfilerConfig(
    framework_profile_params=FrameworkProfile()
)
```

With this `profiler_config` parameter configuration, Debugger calls the default settings of monitoring and profiling\. Debugger monitors system metrics every 500 milliseconds, profiles the fifth step with the detailed profiling option, the seventh step with the data loader profiling option, and the ninth, tenth, and eleventh steps with the Python profiling option\. 

To find available profiling configuration options, the default parameter settings, and examples of how to configure them, see [Start a Training Job with the Default System Monitoring and Customized Framework Profiling with Different Profiling Options](#debugger-configure-framework-profiling-options) and [SageMaker Debugger APIs – FrameworkProfile](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.FrameworkProfile) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

If you want to change the system monitoring interval and enable the default framework profiling, you can specify the `system_monitor_interval_millis` parameter explicitly with the `framework_profile_params` parameter\. For example, to monitor every 1000 milliseconds and enable the default framework profiling, use the following example code\.

```
from sagemaker.debugger import ProfilerConfig, FrameworkProfile
    
profiler_config=ProfilerConfig(
    system_monitor_interval_millis=1000,
    framework_profile_params=FrameworkProfile()
)
```

For more information about the `FrameworkProfile` class, see [SageMaker Debugger APIs – FrameworkProfile](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.FrameworkProfile) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

## Start a Training Job with the Default System Monitoring and Customized Framework Profiling for Target Steps or a Target Time Range<a name="debugger-configure-framework-profiling-range"></a>

If you want to specify target steps or target time intervals to profile your training job, you need to specify parameters for the `FrameworkProfile` class\. The following code examples show how to specify the target ranges for profiling along with system monitoring\.
+ **For a target step range**

  With the following example configuration, Debugger monitors the entire training job every 500 milliseconds \(the default monitoring\) and profiles a target step range from step 5 to step 15 \(for 10 steps\)\.

  ```
  from sagemaker.debugger import ProfilerConfig, FrameworkProfile
      
  profiler_config=ProfilerConfig(
      framework_profile_params=FrameworkProfile(start_step=5, num_steps=10)
  )
  ```

  With the following example configuration, Debugger monitors the entire training job every 1000 milliseconds and profiles a target step range from step 5 to step 15 \(for 10 steps\)\.

  ```
  from sagemaker.debugger import ProfilerConfig, FrameworkProfile
      
  profiler_config=ProfilerConfig(
      system_monitor_interval_millis=1000,
      framework_profile_params=FrameworkProfile(start_step=5, num_steps=10)
  )
  ```
+ **For a target time range**

  With the following example configuration, Debugger monitors the entire training job every 500 milliseconds \(the default monitoring\) and profiles a target time range from the current Unix time for 600 seconds\.

  ```
  import time
  from sagemaker.debugger import ProfilerConfig, FrameworkProfile
  
  profiler_config=ProfilerConfig(
      framework_profile_params=FrameworkProfile(start_unix_time=int(time.time()), duration=600)
  )
  ```

  With the following example configuration, Debugger monitors the entire training job every 1000 milliseconds and profiles a target time range from the current Unix time for 600 seconds\.

  ```
  import time
  from sagemaker.debugger import ProfilerConfig, FrameworkProfile
  
  profiler_config=ProfilerConfig(
      system_monitor_interval_millis=1000,
      framework_profile_params=FrameworkProfile(start_unix_time=int(time.time()), duration=600)
  )
  ```

  The framework profiling is performed for all of the profiling options at the target step or time range\. 

  To find more information about available profiling options, see [SageMaker Debugger APIs – FrameworkProfile](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.FrameworkProfile) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

  The next section show you how to script the available profiling options\.

## Start a Training Job with the Default System Monitoring and Customized Framework Profiling with Different Profiling Options<a name="debugger-configure-framework-profiling-options"></a>

You can use the following profiling configuration classes to micromanage the framework profiling options:
+ [DetailedProfilingConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.DetailedProfilingConfig) – Specify a target step or time range to profile framework operations using the native framework profilers \(TensorFlow profiler and PyTorch profiler\)\. For example, if using TensorFlow, the Debugger hooks enable the TensorFlow profiler to collect TensorFlow\-specific framework metrics\. Detailed profiling enables you to profile all framework operators at a pre\-step \(before the first step\), within steps, and between steps of a training job\.
**Note**  
The detailed profiling might significantly increase GPU memory consumption\. It is not recommended to enable the detailed profiling for more than a couple of steps\.
+ [DataloaderProfilingConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.DataloaderProfilingConfig) – Specify a target step or time range to profile deep learning framework data loader processes\. Debugger collects every data loader event of the frameworks\.
**Note**  
The data loader profiling might lower the training performance while collecting information from data loaders\. We don't recommend that you enable the data loader profiling for more than a couple of steps\.  
Debugger is preconfigured to annotate data loader processes only for the AWS deep learning containers\. Debugger cannot profile data loader processes from any other custom or external training containers\.
+ [PythonProfilingConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.PythonProfilingConfig) – Specify a target step or time range to profile Python functions\. You can also choose between two Python profilers: cProfile and Pyinstrument\.
  + cProfile – The standard Python profiler\. cProfile collects for every Python operator called during training\. With cProfile, Debugger saves cumulative time and annotation of each function call, providing a complete detail of Python functions\. In deep learning, for example, the most frequently called functions might be the convolutional filters and backward pass operators, and cProfile profiles every single of them\. For the cProfile option, you can further select a timer option: total time, CPU time, and off\-CPU time\. While you can profile every function calls executing on processors \(both CPU and GPU\) in CPU time, you can also identify I/O or network bottlenecks with the off\-CPU time option\. The default is total time, and Debugger profiles both CPU and off\-CPU time\. With cProfile, you are able to drill down to every single functions when analyzing the profile data\.
  + Pyinstrument – Pyinstrument is a low overhead Python profiler that works based on sampling\. With the Pyinstrument option, Debugger samples profiling events every millisecond\. Because Pyinstrument measures elapsed wall clock time instead of CPU time, the Pyinstrument option can be a better choice over the cProfile option for reducing profiling noises \(filtering out irrelevant function calls that are cumulatively fast\) and capturing operators that are actually compute intensive \(cumulatively slow\) for training your model\. With Pyinstrument, you are able to see a tree of function calls and better understand the structure and root cause of the slowness\.
**Note**  
Enabling Python profiling might result in slowing down the overall training time\. In case of cProfile, Python operators that are the most frequently called are profiled at every call, so the processing time on profiling increases with respect to the number of calls\. For Pyinstrument, the cumulative profiling time increases with respect to time because of its sampling mechanism\.

The following example configuration shows the full structure when you use the different profiling options with specified values\.

```
import time
from sagemaker.debugger import (ProfilerConfig, 
                                FrameworkProfile, 
                                DetailedProfilingConfig, 
                                DataloaderProfilingConfig, 
                                PythonProfilingConfig)

profiler_config=ProfilerConfig(
    system_monitor_interval_millis=500,
    framework_profile_params=FrameworkProfile(
        detailed_profiling_config=DetailedProfilingConfig(
            start_step=5, 
            num_steps=1
        ),
        dataloader_profiling_config=DataloaderProfilingConfig(
            start_step=7, 
            num_steps=1
        ),
        python_profiling_config=PythonProfilingConfig(
            start_step=9, 
            num_steps=1, 
            python_profiler="cProfile", 
            cprofile_timer="total_time"
        )
    )
)
```

To find more information about available profiling options, see [DetailedProfilingConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.DetailedProfilingConfig), [DataloaderProfilingConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.DataloaderProfilingConfig), and [PythonProfilingConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.PythonProfilingConfig) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.