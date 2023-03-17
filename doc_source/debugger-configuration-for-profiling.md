# Configure Debugger Using Amazon SageMaker Python SDK<a name="debugger-configuration-for-profiling"></a>

By default, SageMaker Debugger monitors resource utilization metrics, such as CPU utilization, GPU utilization, GPU memory utilization, Network, and I/O wait time, of all SageMaker training jobs submitted using the SageMaker Python SDK\. SageMaker Debugger collects these resource utilization metrics every 500 milliseconds\. You don't need to make any additional changes in your code, training script, or job launcher for basic resource utilization tracking purposes\. If you want to check the visualization of the resource utilization metrics of your training job in SageMaker Studio, you can jump onto the [Amazon SageMaker Debugger UI in Amazon SageMaker Studio Experiments](debugger-on-studio.md)\.

If you want to change settings for profiling , you can specify Debugger\-specific parameters while creating a SageMaker training job launcher using SageMaker Python SDK, AWS SDK for Python \(Boto3\), or AWS Command Line Interface \(CLI\)\. In this guide, we focus on how to change profiling options using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. There are two parameters in the [SageMaker estimator classes](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html): `profiler_config` for changing the profiler settings, and `rules` for activating additional analysis tools\.

**Important**  
To use the latest SageMaker Debugger features, you need to upgrade the SageMaker Python SDK and the `SMDebug` client library\. In your iPython kernel, Jupyter Notebook, or JupyterLab environment, run the following code to install the latest versions of the libraries and restart the kernel\.  

```
import sys
import IPython
!{sys.executable} -m pip install -U sagemaker smdebug
IPython.Application.instance().kernel.do_shutdown(True)
```

## Construct a SageMaker Estimator with SageMaker Debugger<a name="debugger-configuration-structure-profiler"></a>

The following code samples are the templates you can start with\. When you construct a SageMaker estimator, add the `profiler_config` and `rules`parameters\. In the subtopic sections of this page, you can find more information about how to configure each parameter\.

**Note**  
The following example codes are not directly executable\. You need to proceed to the next sections and to learn more about how to configure the parameters\.

------
#### [ PyTorch ]

```
# An example of constructing a SageMaker PyTorch estimator
import boto3
import sagemaker
from sagemaker.pytorch import PyTorch
from sagemaker.debugger import ProfilerConfig, ProfilerRule, rule_configs

session=boto3.session.Session()
region=session.region_name

profiler_config=ProfilerConfig(...)
rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInRule())
]

estimator=PyTorch(
    entry_point="directory/to/your_training_script.py",
    role=sagemaker.get_execution_role(),
    base_job_name="debugger-profiling-demo",
    instance_count=1,
    instance_type="ml.p3.2xlarge",
    framework_version="1.12.0",
    py_version="py37",
    
    # SageMaker Debugger parameters
    profiler_config=profiler_config,
    rules=rules
)

estimator.fit(wait=False)
```

------
#### [ TensorFlow ]

```
# An example of constructing a SageMaker TensorFlow estimator
import boto3
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import ProfilerConfig, ProfilerRule, rule_configs

session=boto3.session.Session()
region=session.region_name

profiler_config=ProfilerConfig(...)
rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInRule())
]

estimator=TensorFlow(
    entry_point="directory/to/your_training_script.py",
    role=sagemaker.get_execution_role(),
    base_job_name="debugger-profiling-demo",
    instance_count=1,
    instance_type="ml.p3.2xlarge",
    framework_version="2.8.0",
    py_version="py37",
    
    # SageMaker Debugger parameters
    profiler_config=profiler_config,
    rules=rules
)

estimator.fit(wait=False)
```

------
#### [ MXNet ]

```
# An example of constructing a SageMaker MXNet estimator
import sagemaker
from sagemaker.mxnet import MXNet
from sagemaker.debugger import ProfilerConfig, ProfilerRule, rule_configs

profiler_config=ProfilerConfig(...)
rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInRule())
]

estimator=MXNet(
    entry_point="directory/to/your_training_script.py",
    role=sagemaker.get_execution_role(),
    base_job_name="debugger-profiling-demo",
    instance_count=1,
    instance_type="ml.p3.2xlarge",
    framework_version="1.7.0",
    py_version="py37",
    
    # SageMaker Debugger parameters
    profiler_config=profiler_config,
    rules=rules
)

estimator.fit(wait=False)
```

**Note**  
For MXNet, when configuring the `profiler_config` parameter, you can only configure for system monitoring\. Profiling framework metrics is not supported for MXNet\.

------
#### [ XGBoost ]

```
# An example of constructing a SageMaker XGBoost estimator
import sagemaker
from sagemaker.xgboost.estimator import XGBoost
from sagemaker.debugger import ProfilerConfig, ProfilerRule, rule_configs

profiler_config=ProfilerConfig(...)
rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInRule())
]

estimator=XGBoost(
    entry_point="directory/to/your_training_script.py",
    role=sagemaker.get_execution_role(),
    base_job_name="debugger-profiling-demo",
    instance_count=1,
    instance_type="ml.p3.2xlarge",
    framework_version="1.5-1",

    # Debugger-specific parameters
    profiler_config=profiler_config,
    rules=rules
)

estimator.fit(wait=False)
```

**Note**  
For XGBoost, when configuring the `profiler_config` parameter, you can only configure for system monitoring\. Profiling framework metrics is not supported for XGBoost\.

------
#### [ Generic estimator ]

```
# An example of constructing a SageMaker generic estimator using the XGBoost algorithm base image
import boto3
import sagemaker
from sagemaker.estimator import Estimator
from sagemaker import image_uris
from sagemaker.debugger import ProfilerConfig, DebuggerHookConfig, Rule, ProfilerRule, rule_configs

profiler_config=ProfilerConfig(...)
rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInRule())
]

region=boto3.Session().region_name
xgboost_container=sagemaker.image_uris.retrieve("xgboost", region, "1.5-1")

estimator=Estimator(
    role=sagemaker.get_execution_role()
    image_uri=xgboost_container,
    base_job_name="debugger-demo",
    instance_count=1,
    instance_type="ml.m5.2xlarge",
    
    # Debugger-specific parameters
    profiler_config=profiler_config,
    rules=rules
)

estimator.fit(wait=False)
```

------

The following provides brief descriptions of the parameters\.
+ `profiler_config` – Configure Debugger to collect system metrics and framework metrics from your training job and save into your secured S3 bucket URI or local machine\. You can set how frequently or loosely collect the system metrics\. To learn how to configure the `profiler_config` parameter, see [Configure Debugger for Monitoring Resource Utilization](debugger-configure-system-monitoring.md) and [Configure Debugger for Framework Profiling](debugger-configure-framework-profiling.md)\.
+ `rules` – Configure this parameter to activate SageMaker Debugger built\-in rules that you want to run in parallel\. Make sure that your training job has access to this S3 bucket\. The rules runs on processing containers and automatically analyze your training job to find computational and operational performance issues\. The [ProfilerReport](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#profiler-report) rule is the most integrated rule that runs all built\-in profiling rules and saves the profiling results as a report into your secured S3 bucket\. To learn how to configure the `rules` parameter, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
Debugger securely saves output data in subfolders of your default S3 bucket\. For example, the format of the default S3 bucket URI is `s3://sagemaker-<region>-<12digit_account_id>/<base-job-name>/<debugger-subfolders>/`\. There are three subfolders created by Debugger: `debug-output`, `profiler-output`, and `rule-output`\. You can also retrieve the default S3 bucket URIs using the [SageMaker estimator classmethods](debugger-estimator-classmethods.md)\.

See the following topics to find out how to configure the Debugger\-specific parameters in detail\.

**Topics**
+ [Construct a SageMaker Estimator with SageMaker Debugger](#debugger-configuration-structure-profiler)
+ [Configure Debugger for Monitoring Resource Utilization](debugger-configure-system-monitoring.md)
+ [Configure Debugger for Framework Profiling](debugger-configure-framework-profiling.md)
+ [Updating Debugger System Monitoring and Framework Profiling Configuration while a Training Job is Running](debugger-update-monitoring-profiling.md)
+ [Turn Off Debugger](debugger-turn-off-profiling.md)