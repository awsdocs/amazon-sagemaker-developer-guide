# Configure Debugger Using Amazon SageMaker Python SDK<a name="debugger-configuration-for-profiling"></a>

To configure Debugger to profile your training job and resource utilization, use [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and specify Debugger\-specific parameters while constructing SageMaker estimators\. There are two parameters you need to configure: `profiler_config` and `rules`\.

**Note**  
By default, SageMaker Debugger monitors and debugs SageMaker training jobs without any Debugger\-specific parameters configured in SageMaker estimators\. SageMaker Debugger collects system metrics every 500 milliseconds and basic output tensors \(scalar outputs such as loss and accuracy\) every 500 steps\. It also runs the `ProfilerReport` rule to analyze the system metrics and aggregate the Studio Debugger insights dashboard and a profiling report\. Debugger saves the output data in your secured S3 bucket\.

**Important**  
To use the new SageMaker Debugger features, you need to upgrade the SageMaker Python SDK and the `SMDebug` client library\. In your iPython kernel, Jupyter Notebook, or JupyterLab environment, run the following code to install the latest versions of the libraries and restart the kernel\.  

```
import sys
import IPython
!{sys.executable} -m pip install -U sagemaker smdebug
IPython.Application.instance().kernel.do_shutdown(True)
```

## Construct a SageMaker Estimator with Debugger<a name="debugger-configuration-structure-profiler"></a>

The following example codes show how to construct a SageMaker estimator with the Debugger\-specific parameters depending on a framework of your choice\. Throughout the documentation in the following topics, you can find more information about how to configure the Debugger\-specific parameters that you can mix and match as you want\.

**Note**  
The following example codes are not directly executable\. You need to proceed to the next sections and configure the Debugger\-specific parameters\.

------
#### [ PyTorch ]

To access the deep profiling feature for PyTorch, currently you need to specify the latest AWS deep learning container images with CUDA 11 and later\.

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

To access the deep profiling feature for TensorFlow, currently you need to specify the latest AWS deep learning container images with CUDA 11 and later\.

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

Where you configure the following parameters:
+ `profiler_config` parameter – Configure Debugger to collect system metrics and framework metrics from your training job and save into your secured S3 bucket URI or local machine\. To learn how to configure the `profiler_config` parameter, see [Configure Debugger Monitoring Hardware System Resource Utilization](debugger-configure-system-monitoring.md) and [Configure Debugger Framework Profiling](debugger-configure-framework-profiling.md)\.
+ `rules` parameter – Configure this parameter to enable Debugger built\-in rules that you want to run in parallel\. The rules automatically analyze your training job and find training issues\. The ProfilerReport rule saves the Debugger profiling reports in your secured S3 bucket\. To learn how to configure the `rules` parameter, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
Debugger securely saves output data in subfolders of your default S3 bucket\. For example, the format of the default S3 bucket URI is `s3://sagemaker-<region>-<12digit_account_id>/<base-job-name>/<debugger-subfolders>/`\. There are three subfolders created by Debugger: `debug-output`, `profiler-output`, and `rule-output`\. You can also retrieve the default S3 bucket URIs using the [SageMaker estimator classmethods](debugger-estimator-classmethods.md)\.

See the following topics to find out how to configure the Debugger\-specific parameters in detail\.

**Topics**
+ [Construct a SageMaker Estimator with Debugger](#debugger-configuration-structure-profiler)
+ [Configure Debugger Monitoring Hardware System Resource Utilization](debugger-configure-system-monitoring.md)
+ [Configure Debugger Framework Profiling](debugger-configure-framework-profiling.md)
+ [Updating Debugger System Monitoring and Framework Profiling Configuration while a Training Job is Running](debugger-update-monitoring-profiling.md)
+ [Turn Off Debugger](debugger-turn-off-profiling.md)