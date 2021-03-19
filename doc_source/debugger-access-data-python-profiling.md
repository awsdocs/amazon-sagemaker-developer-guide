# Access the Python Profiling Stats Data<a name="debugger-access-data-python-profiling"></a>

The Python profiling provides framework metrics related to Python functions and operators in your training scripts and the SageMaker deep learning frameworks\. 

<a name="debugger-access-data-python-profiling-modes"></a>**Training Modes and Phases for Python Profiling**

To profile specific intervals during training to partition statistics for each of these intervals, Debugger provides tools to set modes and phases\. 

For training modes, use the following `PythonProfileModes` class:

```
from smdebug.profiler.python_profile_utils import PythonProfileModes
```

This class provides the following options:
+ `PythonProfileModes.TRAIN` – Use if you want to profile the target steps in the training phase\. This mode option available only for TensorFlow\.
+ `PythonProfileModes.EVAL` – Use if you want to profile the target steps in the evaluation phase\. This mode option available only for TensorFlow\.
+ `PythonProfileModes.PREDICT` – Use if you want to profile the target steps in the prediction phase\. This mode option available only for TensorFlow\.
+ `PythonProfileModes.GLOBAL` – Use if you want to profile the target steps in the global phase, which includes the previous three phases\. This mode option available only for PyTorch\.
+ `PythonProfileModes.PRE_STEP_ZERO` – Use if you want to profile the target steps in the initialization stage before the first training step of the first epoch starts\. This phase includes the initial job submission, uploading the training scripts to EC2 instances, preparing the EC2 instances, and downloading input data\. This mode option available for both TensorFlow and PyTorch\.
+ `PythonProfileModes.POST_HOOK_CLOSE` – Use if you want to profile the target steps in the finalization stage after the training job has done and the Debugger hook is closed\. This phase includes profiling data while the training jobs are finalized and completed\. This mode option available for both TensorFlow and PyTorch\.

<a name="debugger-access-data-python-profiling-phases"></a>For training phases, use the following `StepPhase` class:

```
from smdebug.profiler.analysis.utils.python_profile_analysis_utils import StepPhase
```

This class provides the following options:
+ `StepPhase.START` – Use to specify the start point of the initialization phase\.
+ `StepPhase.STEP_START` – Use to specify the start step of the training phase\.
+ `StepPhase.FORWARD_PASS_END` – Use to specify the steps where the forward pass ends\. This option is available only for PyTorch\.
+ `StepPhase.STEP_END` – Use to specify the end steps in the training phase\. This option is available only for TensorFlow\.
+ `StepPhase.END` – Use to specify the ending point of the finalization \(post\-hook\-close\) phase\. If the callback hook is not closed, the finalization phase profiling does not occur\.

**Python Profiling Analysis Tools**

Debugger supports the Python profiling with two profiling tools:
+ cProfile – The standard python profiler\. cProfile collects framework metrics on CPU time for every function called when profiling was enabled\.
+ Pyinstrument – This is a low overhead Python profiler sampling profiling events every milliseconds\.

To learn more about the Python profiling options and what's collected, see [Start a Training Job with the Default System Monitoring and Customized Framework Profiling with Different Profiling Options](debugger-configure-framework-profiling.md#debugger-configure-framework-profiling-options)\.

The following methods of the `PythonProfileAnalysis`, `cProfileAnalysis`, `PyinstrumentAnalysis` classes are provided to fetch and analyze the Python profiling data\. Each function loads the latest data from the default S3 URI\.

```
from smdebug.profiler.analysis.python_profile_analysis import PythonProfileAnalysis, cProfileAnalysis, PyinstrumentAnalysis
```

To set Python profiling objects for analysis, use the cProfileAnalysis or PyinstrumentAnalysis classes as shown in the following example code\. It shows how to set a `cProfileAnalysis` object, and if you want to use `PyinstrumentAnalysis`, replace the class name\.

```
python_analysis = cProfileAnalysis(
    local_profile_dir=tf_python_stats_dir, 
    s3_path=tj.profiler_s3_output_path
)
```

The following methods are available for the `cProfileAnalysis` and `PyinstrumentAnalysis` classes to fetch the Python profiling stats data:
+ `python_analysis.fetch_python_profile_stats_by_time(start_time_since_epoch_in_secs, end_time_since_epoch_in_secs)` – Takes in a start time and end time, and returns the function stats of step stats whose start or end times overlap with the provided interval\.
+ `python_analysis.fetch_python_profile_stats_by_step(start_step, end_step, mode, start_phase, end_phase)` – Takes in a start step and end step and returns the function stats of all step stats whose profiled `step` satisfies `start_step <= step < end_step`\. 
  + `start_step` and `end_step` \(str\) – Specify the start step and end step to fetch the Python profiling stats data\.
  + `mode` \(str\) – Specify the mode of training job using the `PythonProfileModes` enumerator class\. The default is `PythonProfileModes.TRAIN`\. Available options are provided in the [Training Modes and Phases for Python Profiling](#debugger-access-data-python-profiling-modes) section\.
  + `start_phase` \(str\) – Specify the start phase in the target step\(s\) using the `StepPhase` enumerator class\. This parameter enables profiling between different phases of training\. The default is `StepPhase.STEP_START`\. Available options are provided in the [ Training Modes and Phases for Python Profiling](#debugger-access-data-python-profiling-phases) section\.
  + `end_phase` \(str\) – Specify the end phase in the target step\(s\) using the `StepPhase` enumerator class\. This parameter sets up the end phase of training\. Available options are as same as the ones for the `start_phase` parameter\. The default is `StepPhase.STEP_END`\. Available options are provided in the [ Training Modes and Phases for Python Profiling](#debugger-access-data-python-profiling-phases) section\.
+ `python_analysis.fetch_profile_stats_between_modes(start_mode, end_mode)` – Fetches stats from the Python profiling between the start and end modes\.
+ `python_analysis.fetch_pre_step_zero_profile_stats()` – Fetches the stats from the Python profiling until step 0\.
+ `python_analysis.fetch_post_hook_close_profile_stats()` – Fetches stats from the Python profiling after the hook is closed\.
+ `python_analysis.list_profile_stats()` – Returns a DataFrame of the Python profiling stats\. Each row holds the metadata for each instance of profiling and the corresponding stats file \(one per step\)\.
+ `python_analysis.list_available_node_ids()` – Returns a list the available node IDs for the Python profiling stats\.

The `cProfileAnalysis` class specific methods:
+  `fetch_profile_stats_by_training_phase()` – Fetches and aggregates the Python profiling stats for every possible combination of start and end modes\. For example, if a training and validation phases are done while detailed profiling is enabled, the combinations are `(PRE_STEP_ZERO, TRAIN)`, `(TRAIN, TRAIN)`, `(TRAIN, EVAL)`, `(EVAL, EVAL)`, and `(EVAL, POST_HOOK_CLOSE)`\. All stats files within each of these combinations are aggregated\.
+  `fetch_profile_stats_by_job_phase()` – Fetches and aggregates the Python profiling stats by job phase\. The job phases are `initialization` \(profiling until step 0\), `training_loop` \(training and validation\), and `finalization` \(profiling after the hook is closed\)\.