# List of Debugger Built\-in Rules<a name="debugger-built-in-rules"></a>

Use the Debugger built\-in rules provided by Amazon SageMaker Debugger and analyze tensors emitted while training your models\. The Debugger built\-in rules monitor various common conditions that are critical for the success of a training job\. You can call the built\-in rules using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) or the low\-level SageMaker API operations\. Depending on deep learning frameworks of your choice, there are four scopes of validity for the built\-in rules as shown in the following table\.

**Note**  
The maximum numbers of built\-in rules for a training job are 20 for `ProfilerRule` and 20 for `Rule`\. SageMaker Debugger fully manages the built\-in rules and analyzes your training job in parallel\. For more information about billing, see the **Amazon SageMaker Studio is available at no additional charge** section of the [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/) page\.

**Important**  
To use the new Debugger features, you need to upgrade the SageMaker Python SDK and the SMDebug client library\. In your iPython kernel, Jupyter notebook, or JupyterLab environment, run the following code to install the latest versions of the libraries and restart the kernel\.  

```
import sys
import IPython
!{sys.executable} -m pip install -U sagemaker smdebug
IPython.Application.instance().kernel.do_shutdown(True)
```

## Debugger ProfilerRule<a name="debugger-built-in-rules-ProfilerRule"></a>

The following rules are the Debugger built\-in rules that are callable using the `ProfilerRule.sagemaker` classmethod\.


**Debugger Built\-in Rules for Generating Profiling Reports**  

| Scope of Validity | Built\-in Rules | 
| --- | --- | 
| Profiling Report for any SageMaker training job |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 


**Debugger Built\-in Rules for Monitoring Hardware System Resource Utilization \(System Metrics\)**  

| Scope of Validity | Built\-in Rules | 
| --- | --- | 
| Generic system monitoring rules for any SageMaker training job |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 


**Debugger Built\-in Rules for Profiling Framework Metrics**  

| Scope of Validity | Built\-in Rules | 
| --- | --- | 
| Profiling rules for deep learning frameworks \(TensorFlow and PyTorch\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 

## Debugger Rule<a name="debugger-built-in-rules-Rule"></a>

The following rules are the Debugger built\-in rules that are callable using the `Rule.sagemaker` classmethod\.


**Debugger Built\-in Rules for Generating Training Reports**  

| Scope of Validity | Built\-in Rules | 
| --- | --- | 
| Training Report for SageMaker XGboost training job |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 


**Debugger Built\-in Rules for Debugging Model Training Data \(Output Tensors\)**  

| Scope of Validity | Built\-in Rules | 
| --- | --- | 
| Deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 
| Deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) and the XGBoost algorithm  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 
| Deep learning applications |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 
| XGBoost algorithm |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html)  | 

**To use the built\-in rules with default parameter values** – use the following configuration format:

```
from sagemaker.debugger import Rule, ProfilerRule, rule_configs

rules = [
    ProfilerRule.sagemaker(rule_configs.BuiltInRuleName_1()),
    ProfilerRule.sagemaker(rule_configs.BuiltInRuleName_2()),
    ...
    ProfilerRule.sagemaker(rule_configs.BuiltInRuleName_n()),
    Rule.sagemaker(rule_configs.built_in_rule_name_1()),
    Rule.sagemaker(rule_configs.built_in_rule_name_2()),
    ...
    Rule.sagemaker(rule_configs.built_in_rule_name_n())
]
```

**To use the built\-in rules with customizing the parameter values** – use the following configuration format:

```
from sagemaker.debugger import Rule, ProfilerRule, rule_configs

rules = [
    ProfilerRule.sagemaker(
        base_config=rule_configs.BuiltInRuleName(),
        rule_parameters={
                "key": "value"
        }
    )
    Rule.sagemaker(
        base_config=rule_configs.built_in_rule_name(),
        rule_parameters={
                "key": "value"
        }
        collections_to_save=[ 
            CollectionConfig(
                name="tensor_collection_name", 
                parameters={
                    "key": "value"
                } 
            )
        ]
    )
]
```

To find available keys for the `rule_parameters` parameter, see the parameter description tables\.

Sample rule configuration codes are provided for each built\-in rule below the parameter description tables\.
+ For a full instruction and examples of using the Debugger built\-in rules, see [Debugger Built\-in Rules Example Code](use-debugger-built-in-rules.md#debugger-deploy-built-in-rules)\.
+ For a full instruction of using the built\-in rules with the low\-level SageMaker API operations, see [Add Debugger Built\-in Rule Configuration to the `CreateTrainingJob` API Operation](debugger-createtrainingjob-api.md#debugger-built-in-rules-api)\.

## ProfilerReport<a name="profiler-report"></a>

The ProfilerReport rule invokes all of the built\-in rules for monitoring and profiling\. It creates a profiling report and updates when the individual rules are triggered\. You can download a comprehensive profiling report while a training job is running or after the training job is complete\. You can adjust the rule parameter values to customize sensitivity of the built\-in monitoring and profiling rules\. The following example code shows the basic format to adjust the built\-in rule parameters through the ProfilerReport rule\.

```
rules=[
    ProfilerRule.sagemaker(
        rule_configs.ProfilerReport(
            <BuiltInRuleName>_<parameter_name> = value
        )
    )  
]
```

If you trigger this ProfilerReport rule without any customized parameter as shown in the following example code, then the ProfilerReport rule triggers all of the built\-in rules for monitoring and profiling with their default parameter values\.

```
rules=[ProfilerRule.sagemaker(rule_configs.ProfilerReport())]
```

The following example code shows how to specify and adjust the CPUBottleneck rule's `cpu_threshold` parameter and the IOBottleneck rule's `threshold` parameter\.

```
rules=[
    ProfilerRule.sagemaker(
        rule_configs.ProfilerReport(
            CPUBottleneck_cpu_threshold = 90,
            IOBottleneck_threshold = 90
        )
    )  
]
```




**Parameter Descriptions for the OverallSystemUsage Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| <BuiltInRuleName>\_<parameter\_name> |  Customizable parameter to adjust thresholds of other built\-in monitoring and profiling rules\.  **Optional** Default value: `None`  | 

## BatchSize<a name="batch-size-rule"></a>

The BatchSize rule helps detect if GPU is underutilized due to a small batch size\. To detect this issue, this rule monitors the average CPU utilization, GPU utilization, and GPU memory utilization\. If utilization on CPU, GPU, and GPU memory is low on average, it may indicate that the training job can either run on a smaller instance type or can run with a bigger batch size\. This analysis does not work for frameworks that heavily overallocate memory\. However, increasing the batch size can lead to processing or data loading bottlenecks because more data preprocessing time is required in each iteration\.


**Parameter Descriptions for the BatchSize Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| cpu\_threshold\_p95 |  Defines the threshold for 95th quantile of CPU utilization in percentage\. **Optional** Valid values: Integer Default value: `70` \(in percentage\)  | 
| gpu\_threshold\_p95 |  Defines the threshold for 95th quantile of GPU utilization in percentage\. **Optional** Valid values: Integer Default value: `70` \(in percentage\)  | 
| gpu\_memory\_threshold\_p95 | Defines the threshold for 95th quantile of GPU memory utilization in percentage\. **Optional** Valid values: Integer Default values: `70` \(in percentage\)  | 
| patience | Defines the number of data points to skip until the rule starts evaluation\. The first several steps of training jobs usually show high volume of data processes, so keep the rule patient and prevent it from being invoked too soon with a given number of profiling data that you specify with this parameter\. **Optional** Valid values: Integer Default values: `100`  | 
| window |  Window size for computing quantiles\. **Optional** Valid values: Integer Default values: `500`  | 
| scan\_interval\_us |  Time interval that timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## CPUBottleneck<a name="cpu-bottleneck"></a>

The CPUBottleneck rule helps detect if GPU is underutilized due to CPU bottlenecks\. Rule returns True if number of CPU bottlenecks exceeds a predefined threshold\.


**Parameter Descriptions for the CPUBottleneck Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| threshold |  Defines the threshold for proportion of bottlenecked time to the total training time\. If the proportion exceeds the percentage specified to the threshold parameter, the rule switches the rule status to True\. **Optional** Valid values: Integer Default value: `50` \(in percentage\)  | 
| gpu\_threshold |  A threshold that defines low GPU utilization\. **Optional** Valid values: Integer Default value: `10` \(in percentage\)  | 
| cpu\_threshold | A threshold that defines high CPU utilization\. **Optional** Valid values: Integer Default values: `90` \(in percentage\)  | 
| patience | Defines the number of data points to skip until the rule starts evaluation\. The first several steps of training jobs usually show high volume of data processes, so keep the rule patient and prevent it from being invoked too soon with a given number of profiling data that you specify with this parameter\. **Optional** Valid values: Integer Default values: `100`  | 
| scan\_interval\_us | Time interval with which timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## GPUMemoryIncrease<a name="gpu-memory-increase"></a>

The GPUMemoryIncrease rule helps detect a large increase in memory usage on GPUs\.


**Parameter Descriptions for the GPUMemoryIncrease Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| increase |  Defines the threshold for absolute memory increase\. **Optional** Valid values: Integer Default value: `10` \(in percentage\)  | 
| patience |  Defines the number of data points to skip until the rule starts evaluation\. The first several steps of training jobs usually show high volume of data processes, so keep the rule patient and prevent it from being invoked too soon with a given number of profiling data that you specify with this parameter\. **Optional** Valid values: Integer Default values: `100`  | 
| window |  Window size for computing quantiles\. **Optional** Valid values: Integer Default values: `500`  | 
| scan\_interval\_us |  Time interval that timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## IOBottleneck<a name="io-bottleneck"></a>

This rule helps to detect if GPU is underutilized due to data IO bottlenecks\. Rule returns True if number of IO bottlenecks exceeds a predefined threshold\.


**Parameter Descriptions for the IOBottleneck Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| threshold | Defines the threshold when Rule to return True\.**Optional**Valid values: IntegerDefault value: `50` \(in percentage\) | 
| gpu\_threshold |  A threshold that defines when GPU is considered underutilized\. **Optional** Valid values: Integer Default value: `70` \(in percentage\)  | 
| io\_threshold | A threshold that defines high IO wait time\.**Optional**Valid values: IntegerDefault values: `50` \(in percentage\) | 
| patience | Defines the number of data points to skip until the rule starts evaluation\. The first several steps of training jobs usually show high volume of data processes, so keep the rule patient and prevent it from being invoked too soon with a given number of profiling data that you specify with this parameter\.**Optional**Valid values: IntegerDefault values: `1000` | 
| scan\_interval\_us |  Time interval that timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## LoadBalancing<a name="load-balancing"></a>

The LoadBalancing rule helps detect issues in workload balancing among multiple GPUs\.


**Parameter Descriptions for the LoadBalancing Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| threshold |  Defines the workload percentage\. **Optional** Valid values: Integer Default value: `0.5` \(unitless proportion\)  | 
| patience |  Defines the number of data points to skip until the rule starts evaluation\. The first several steps of training jobs usually show high volume of data processes, so keep the rule patient and prevent it from being invoked too soon with a given number of profiling data that you specify with this parameter\. **Optional** Valid values: Integer Default values: `10`  | 
| scan\_interval\_us |  Time interval that timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## LowGPUUtilization<a name="low-gpu-utilization"></a>

The LowGPUUtilization rule helps detect if GPU utilization is low or suffers from fluctuations\. This is checked for each GPU on each worker\. Rule returns True if 95th quantile is below threshold\_p95 which indicates underutilization\. Rule returns true if 95th quantile is above threshold\_p95 and 5th quantile is below threshold\_p5 which indicates fluctuations\.


**Parameter Descriptions for the LowGPUUtilization Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| threshold\_p95 |  A threshold for 95th quantile below which GPU is considered to be underutilized\. **Optional** Valid values: Integer Default value: `70` \(in percentage\)  | 
| threshold\_p5 | A threshold for 5th quantile\. Default is 10 percent\.**Optional**Valid values: IntegerDefault values: `10` \(in percentage\) | 
| patience |  Defines the number of data points to skip until the rule starts evaluation\. The first several steps of training jobs usually show high volume of data processes, so keep the rule patient and prevent it from being invoked too soon with a given number of profiling data that you specify with this parameter\. **Optional** Valid values: Integer Default values: `1000`  | 
| window |  Window size for computing quantiles\. **Optional** Valid values: Integer Default values: `500`  | 
| scan\_interval\_us |  Time interval that timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## OverallSystemUsage<a name="overall-system-usage"></a>

The OverallSystemUsage rule measures overall system usage per worker node\. The rule currently only aggregates values per node and computes their percentiles\.


**Parameter Descriptions for the OverallSystemUsage Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| scan\_interval\_us |  Time interval to scan timeline files\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## MaxInitializationTime<a name="max-initialization-time"></a>

The MaxInitializationTime rule helps detect if the training initialization is taking too much time\. The rule waits until the first step is available\.


**Parameter Descriptions for the MaxInitializationTime Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| threshold |  Defines the threshold in minutes to wait for the first step to become available\. **Optional** Valid values: Integer Default value: `20` \(in minutes\)  | 
| scan\_interval\_us |  Time interval with which timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## OverallFrameworkMetrics<a name="overall-framework-metrics"></a>

The OverallFrameworkMetrics rule summarizes the time spent on framework metrics, such as forward and backward pass, and data loading\.


**Parameter Descriptions for the OverallFrameworkMetrics Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| scan\_interval\_us |  Time interval to scan timeline files\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## StepOutlier<a name="step-outlier"></a>

The StepOutlier rule helps detect outliers in step durations\. This rule returns `True` if there are outliers with step durations larger than `stddev` sigmas of the entire step durations in a time range\.


**Parameter Descriptions for the StepOutlier Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 
| stddev |  Defines a factor by which to multiply the standard deviation\. For example, the rule is invoked by default when a step duration is larger or smaller than 5 times the standard deviation\.  **Optional** Valid values: Integer Default value: `5` \(in minutes\)  | 
| mode | Mode under which steps have been saved and on which Rule should run on\. Per default rule will run on steps from EVAL and TRAIN phase**Optional**Valid values: IntegerDefault value: `5` \(in minutes\) | 
| n\_outliers | How many outliers to ignore before rule returns True**Optional**Valid values: IntegerDefault value: `10` | 
| scan\_interval\_us |  Time interval with which timeline files are scanned\. **Optional** Valid values: Integer Default values: `60000000` \(in microseconds\)  | 

## CreateXgboostReport<a name="create-xgboost-report"></a>

The CreateXgboostReport rule collects output tensors from an XGBoost training job and autogenerates a comprehensive training report\. You can download a comprehensive profiling report while a training job is running or after the training job is complete, and check progress of training or the final result of the training job\. The CreateXgboostReport rule collects the following output tensors by default: 
+ `hyperparameters` – Saves at the first step
+ `metrics` – Saves loss and accuracy every 5 steps
+ `feature_importance` – Saves every 5 steps
+ `predictions` – Saves every 5 steps
+ `labels` – Saves every 5 steps


**Parameter Descriptions for the CreateXgboostReport Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\. **Required** Valid values: String  | 

```
rules=[
    Rule.sagemaker(
        rule_configs.create_xgboost_report()
    )  
]
```

## DeadRelu<a name="dead-relu"></a>

This rule detects when the percentage of rectified linear unit \(ReLU\) activation functions in a trial are considered dead because their activation activity has dropped below a threshold\. If the percent of inactive ReLUs in a layer is greater than the `threshold_layer` value of inactive ReLUs, the rule returns `True`\.


**Parameter Descriptions for the DeadRelu Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `".*relu_output"`  | 
| threshold\_inactivity |  Defines a level of activity below which a ReLU is considered to be dead\. A ReLU might be active in the beginning of a trial and then slowly die during the training process\. If the ReLU is active less than the `threshold_inactivity`, it is considered to be dead\. **Optional** Valid values: Float Default values: `1.0` \(in percentage\)  | 
| threshold\_layer |  Returns `True` if the percentage of inactive ReLUs in a layer is greater than `threshold_layer`\. Returns `False` if the percentage of inactive ReLUs in a layer is less than `threshold_layer`\. **Optional** Valid values: Float Default values: `50.0` \(in percentage\)  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.dead_relu(),
        rule_parameters={
                "tensor_regex": ".*relu_output|.*ReLU_output",
                "threshold_inactivity": "1.0",
                "threshold_layer": "50.0"
        }
        collections_to_save=[ 
            CollectionConfig(
                name="custom_relu_collection", 
                parameters={
                    "include_regex: ".*relu_output|.*ReLU_output",
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## ExplodingTensor<a name="exploding-tensor"></a>

This rule detects whether the tensors emitted during training have non\-finite values, either infinite or NaN \(not a number\)\. If a non\-finite value is detected, the rule returns `True`\.


**Parameter Descriptions for the ExplodingTensor Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: String Default value: `None`  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: String  Default value: `None`  | 
| only\_nan |   `True` to monitor the `base_trial` tensors only for `NaN` values and not for infinity\.  `False` to treat both `NaN` and infinity as exploding values and to monitor for both\. **Optional** Default value: `False`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.exploding_tensor(),
        rule_parameters={
                "tensor_regex": ".*gradient",
                "only_nan": "False"
        }
        collections_to_save=[ 
            CollectionConfig(
                name="gradients", 
                parameters={
                    "save_interval": "500"
                }
            )
        ]
    )
]
```

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## PoorWeightInitialization<a name="poor-weight-initialization"></a>

 This rule detects if your model parameters have been poorly initialized\. 

Good initialization breaks the symmetry of the weights and gradients in a neural network and maintains commensurate activation variances across layers\. Otherwise, the neural network doesn't learn effectively\. Initializers like Xavier aim to keep variance constant across activations, which is especially relevant for training very deep neural nets\. Too small an initialization can lead to vanishing gradients\. Too large an initialization can lead to exploding gradients\. This rule checks the variance of activation inputs across layers, the distribution of gradients, and the loss convergence for the initial steps to determine if a neural network has been poorly initialized\.


**Parameter Descriptions for the PoorWeightInitialization Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| activation\_inputs\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: String Default value: `".*relu_input"`  | 
| threshold |  If the ratio between minimum and maximum variance of weights per layer exceeds the `threshold` at a step, the rule returns `True`\. **Optional** Valid values: Float Default value: `10.0`  | 
| distribution\_range |  If the minimum difference between 5th and 95th percentiles of the gradient distribution is less than the `distribution_range`, the rule returns `True`\. **Optional** Valid values: Float Default value: `0.001`  | 
| patience |  The number of steps to wait until the loss is considered to be no longer decreasing\. **Optional** Valid values: Integer Default value: `5`  | 
| steps |  The number of steps this rule analyzes\. You typically need to check only the first few iterations\. **Optional** Valid values: Float Default value: `10`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.poor_weight_initialization(),
        rule_parameters={
                "activation_inputs_regex": ".*relu_input|.*ReLU_input",
                "threshold": "10.0",
                "distribution_range": "0.001",
                "patience": "5",
                "steps": "10"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="custom_relu_collection", 
                parameters={
                    "include_regex": ".*relu_input|.*ReLU_input",
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## SaturatedActivation<a name="saturated-activation"></a>

This rule detects if the tanh and sigmoid activation layers are becoming saturated\. An activation layer is saturated when the input of the layer is close to the maximum or minimum of the activation function\. The minimum and maximum of the tanh and sigmoid activation functions are defined by their respective `min_threshold` and `max_thresholds` values\. If the activity of a node drops below the `threshold_inactivity` percentage, it is considered saturated\. If more than a `threshold_layer` percent of the nodes are saturated, the rule returns `True`\.


**Parameter Descriptions for the SaturatedActivation Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: String  Default value: `".*tanh_input\|.*sigmoid_input".`  | 
| threshold\_tanh\_min |  The minimum and maximum thresholds that define the extremes of the input for a tanh activation function, defined as: `(min_threshold, max_threshold)`\. The default values are determined based on a vanishing gradient threshold of 0\.0000001\. **Optional** Valid values: Float Default values: `-9.4999`  | 
| threshold\_tanh\_max |  The minimum and maximum thresholds that define the extremes of the input for a tanh activation function, defined as: `(min_threshold, max_threshold)`\. The default values are determined based on a vanishing gradient threshold of 0\.0000001\. **Optional** Valid values: Float Default values: `9.4999`  | 
| threshold\_sigmoid\_min |  The minimum and maximum thresholds that define the extremes of the input for a sigmoid activation function, defined as: `(min_threshold, max_threshold)`\. The default values are determined based on a vanishing gradient threshold of 0\.0000001\. **Optional** Valid values: Float Default values: `-23`  | 
| threshold\_sigmoid\_max |  The minimum and maximum thresholds that define the extremes of the input for a sigmoid activation function, defined as: `(min_threshold, max_threshold)`\. The default values are determined based on a vanishing gradient threshold of 0\.0000001\. **Optional** Valid values: Float Default values: `16.99999`  | 
| threshold\_inactivity |  The percentage of inactivity below which the activation layer is considered to be saturated\. The activation might be active in the beginning of a trial and then slowly become less active during the training process\. **Optional** Valid values: Float Default values: `1.0`  | 
| threshold\_layer |  Returns `True` if the number of saturated activations in a layer is greater than the `threshold_layer` percentage\. Returns `False` if the number of saturated activations in a layer is less than the `threshold_layer` percentage\. **Optional** Valid values: Float Default values: `50.0`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.saturated_activation(),
        rule_parameters={
                "tensor_regex": ".*tanh_input|.*sigmoid_input",
                "threshold_tanh_min": "-9.4999",
                "threshold_tanh_max": "9.4999",
                "threshold_sigmoid_min": "-23",
                "threshold_sigmoid_max": "16.99999",
                "threshold_inactivity": "1.0",
                "threshold_layer": "50.0"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="custom_activations_collection",
                parameters={
                    "include_regex": ".*tanh_input|.*sigmoid_input"
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## VanishingGradient<a name="vanishing-gradient"></a>

This rule detects if the gradients in a trial become extremely small or drop to a zero magnitude\. If the mean of the absolute values of the gradients drops below a specified `threshold`, the rule returns `True`\.


**Parameters Descriptions for the VanishingGradient Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| threshold | The value at which the gradient is determined to be vanishing\.**Optional**Valid values: FloatDefault value: `0.0000001`\. | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.vanishing_gradient(),
        rule_parameters={
                "threshold": "0.0000001"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="gradients", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## WeightUpdateRatio<a name="weight-update-ratio"></a>

This rule keeps track of the ratio of updates to weights during training and detects if that ratio gets too large or too small\. If the ratio of updates to weights is larger than the `large_threshold value` or if this ratio is smaller than `small_threshold`, the rule returns `True`\.

Conditions for training are best when the updates are commensurate to gradients\. Excessively large updates can push the weights away from optimal values, and very small updates result in very slow convergence\. This rule requires weights to be available for two training steps, and `train.save_interval` needs to be set equal to `num_steps`\.


**Parameter Descriptions for the WeightUpdateRatio Rule**  

| Parameter Name, | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| num\_steps |  The number of steps across which the rule checks to determine if the tensor has changed\.  The number of steps across which you want to compare the weight ratios\. If you pass no value, the rule runs by default against the current step and the immediately previous saved step\. If you override the default by passing a value for this parameter, the comparison is done between weights at step `s` and at a step `>= s - num_steps`\. **Optional** Valid values: Integer Default value: `None`  | 
| large\_threshold |  The maximum value that the ratio of updates to weight can take before the rule returns `True`\.  **Optional** Valid values: Float Default value: `10.0`  | 
| small\_threshold |  The minimum value that the ratio of updates to weight can take, below which the rule returns `True`\. **Optional** Valid values: Float Default value: `0.00000001`  | 
| epsilon |  A small constant used to ensure that Debugger does not divide by zero when computing the ratio updates to weigh\. **Optional** Valid values: Float Default value: `0.000000001`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.weight_update_ratio(),
        rule_parameters={
                "num_steps": "100",
                "large_threshold": "10.0",
                "small_threshold": "0.00000001",
                "epsilon": "0.000000001"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="weights", 
                parameters={
                    "train.save_interval": "100"
                } 
            )
        ]
    )
]
```

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## AllZero<a name="all-zero"></a>

This rule detects if all or a specified percentage of the tensor values are zero\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameters Descriptions for the AllZero Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| threshold |  Specifies the percentage of values in the tensor that needs to be zero for this rule to be invoked\.  **Optional** Valid values: Float Default value: 100 \(in percentage\)  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.all_zero(),
        rule_parameters={
                "tensor_regex": ".*",
                "threshold": "100"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="all", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## ClassImbalance<a name="class-imbalance"></a>

This rule measures sampling imbalances between classes and throws errors if the imbalance exceeds a threshold or if too many mispredictions for underrepresented classes occur as a result of the imbalance\.

Classification models require well\-balanced classes in the training dataset or a proper weighting/sampling of classes during training\. The rule performs the following checks:
+  It counts the occurrences per class\. If the ratio of number of samples between smallest and largest class is larger than the `threshold_imbalance`, an error is thrown\.
+  It checks the prediction accuracy per class\. If resampling or weighting has not been correctly applied, then the model can reach high accuracy for the class with many training samples, but low accuracy for the classes with few training samples\. If a fraction of mispredictions for a certain class is above `threshold_misprediction`, an error is thrown\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the ClassImbalance Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| threshold\_imbalance |  The acceptable imbalance between the number of samples in the smallest class and in the largest class\. Exceeding this threshold value throws an error\. **Optional** Valid values: Float Default value: `10`  | 
| threshold\_misprediction |  A limit on the fraction of mispredictions allowed for each class\. Exceeding this threshold throws an error\. The underrepresented classes are most at risk of crossing this threshold\.  **Optional** Valid values: Float Default value: `0.7`  | 
| samples |  The number of labels that have to be processed before an imbalance is evaluated\. The rule might not be triggered until it has seen sufficient samples across several steps\. The more classes that your dataset contains, the larger this `sample` number should be\.  **Optional** Valid values: Integer Default value: `500` \(assuming a dataset like MNIST with 10 classes\)  | 
| argmax |  If `True`, [np\.argmax]( https://docs.scipy.org/doc/numpy-1.9.3/reference/generated/numpy.argmax.html) is applied to the prediction tensor\. Required when you have a vector of probabilities for each class\. It is used to determine which class has the highest probability\. **Conditional** Valid values: Boolean Default value: `False`  | 
| labels\_regex |  The name of the tensor that contains the labels\. **Optional** Valid values: String Default value: `".*labels"`  | 
| predictions\_regex |  The name of the tensor that contains the predictions\. **Optional** Valid values: String Default value: `".*predictions"`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.class_imbalance(),
        rule_parameters={
                "threshold_imbalance": "10",
                "threshold_misprediction": "0.7",
                "samples": "500",
                "argmax": "False",
                "labels_regex": ".*labels",
                "predictions_regex": ".*predictions"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="custom_output_collection",
                parameters={
                    "include_regex": ".*labels|.*predictions",
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## LossNotDecreasing<a name="loss-not-decreasing"></a>

This rule detects when the loss is not decreasing in value at an adequate rate\. These losses must be scalars\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the LossNotDecreasing Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| tensor\_regex |  A list of regex patterns that is used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `None`  | 
| use\_losses\_collection |  If set to `True`, looks for losses in the collection named "losses" when the collection is present\. **Optional** Valid values: Boolean Default value: `True`  | 
| num\_steps |  The minimum number of steps after which the rule checks if the loss has decreased\. Rule evaluation happens every `num_steps`\. The rule compares the loss for this step with the loss at a step which is at least `num_steps` behind the current step\. For example, suppose that the loss is being saved every three steps, but `num_steps` is set to 10\. At step 21, loss for step 21 is compared with loss for step 9\. The next step at which loss is checked is step 33, because ten steps after step 21 is step 31, and at step 31 and step 32 loss is not saved\.  **Optional** Valid values: Integer Default value: `10`  | 
| diff\_percent |  The minimum percentage difference by which the loss should decrease between `num_steps`\. **Optional** Valid values: `0.0` < float < `100` Default value: `0.1` \(in percentage\)  | 
| increase\_threshold\_percent |  The maximum threshold percent that loss is allowed to increase in case loss has been increasing **Optional** Valid values: `0` < float < `100` Default value: `5` \(in percentage\)  | 
| mode |  The name of the Debugger mode to query tensor values for rule checking\. If this is not passed, the rule checks in order by default for the `mode.EVAL`, then `mode.TRAIN`, and then `mode.GLOBAL`\.  **Optional** Valid values: String \(`EVAL`, `TRAIN`, or `GLOBAL`\) Default value: `GLOBAL`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.loss_not_decreasing(),
        rule_parameters={
                "tensor_regex": ".*",
                "use_losses_collection": "True",
                "num_steps": "10",
                "diff_percent": "0.1",
                "increase_threshold_percent": "5",
                "mode": "GLOBAL"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="losses", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## Overfit<a name="overfit"></a>

This rule detects if your model is being overfit to the training data by comparing the validation and training losses\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
A standard way to prevent overfitting is to regularize your model\.


**Parameter Descriptions for the Overfit Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| start\_step |  The step from which to start comparing the validation and training loss\. **Optional** Valid values: Integer Default value: `0`  | 
| patience |  The number of steps for which the `ratio_threshold` is allowed to exceed the value set before the model is considered to be overfit\. **Optional** Valid values: Integer Default value: `1`  | 
| ratio\_threshold |  The maximum ratio of the difference between the mean validation loss and mean training loss to the mean training loss\. If this threshold is exceeded for a `patience` number of steps, the model is being overfit and the rule returns `True`\. **Optional** Valid values: Float Default value: `0.1`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.overfit(),
        rule_parameters={
                "tensor_regex": ".*",
                "start_step": "0",
                "patience": "1",
                "ratio_threshold": "0.1"
        },
        collections_to_save=[
            CollectionConfig(
                name="losses", 
                parameters={
                    "train.save_interval": "100",
                    "eval.save_interval": "10"
                } 
            )
        ]
    )
]
```

## Overtraining<a name="overtraining"></a>

This rule detects if a model is being overtrained\. After a number of training iterations on a well\-behaved model \(both training and validation loss decrease\), the model approaches to a minimum of the loss function and does not improve anymore\. If the model continues training it can happen that validation loss starts increasing, because the model starts overfitting\. This rule sets up thresholds and conditions to determine if the model is not improving, and prevents overfitting problems due to overtraining\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.

**Note**  
Overtraining can be avoided by early stopping\. For information on early stopping, see [Stop Training Jobs Early](automatic-model-tuning-early-stopping.md)\. For an example that shows how to use spot training with Debugger, see [Enable Spot Training with Amazon SageMaker Debugger](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/mxnet_spot_training/mxnet-spot-training-with-sagemakerdebugger.ipynb)\. 


**Parameter Descriptions for the Overtraining Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| patience\_train |  The number of steps to wait before the training loss is considered to not to be improving anymore\. **Optional** Valid values: Integer Default value: `5`  | 
| patience\_validation | The number of steps to wait before the validation loss is considered to not to be improving anymore\.**Optional**Valid values: IntegerDefault value: `10` | 
| delta |  The minimum threshold by how much the error should improve before it is considered as a new optimum\. **Optional** Valid values: Float Default value: `0.01`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.overtraining(),
        rule_parameters={
                "patience_train": "5",
                "patience_validation": "10",
                "delta": "0.01"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="losses", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## SimilarAcrossRuns<a name="similar-across-runs"></a>

This rule compares tensors gathered from a base trial with tensors from another trial\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the SimilarAcrossRuns Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| other\_trial |  A completed training job name whose tensors you want to compare to those tensors gathered from the current `base_trial`\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.similar_across_runs(),
        rule_parameters={
                "other_trial": "<specify-another-job-name>",
                "collection_names": "losses",
                "tensor_regex": ".*"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="losses", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## StalledTrainingRule<a name="stalled-training"></a>

StalledTrainingRule detects if there is no progress made on training job, and stops the training job if the rule fires\. This rule requires tensors to be periodically saved in a time interval defined by its `threshold` parameter\. This rule keeps on monitoring for new tensors, and if no new tensor has been emitted for threshold interval rule gets fired\. 


**Parameter Descriptions for the StalledTrainingRule Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| threshold |  A threshold that defines by how much time in seconds the rule waits for a tensor output until it fires a stalled training issue\. Default value is 1800 seconds\. **Optional** Valid values: Integer Default value: `1800`  | 
| stop\_training\_on\_fire |  If set to `True`, watches if the base training job outputs tensors in "`threshold`" seconds\. **Optional** Valid values: Boolean Default value: `False`  | 
| training\_job\_name\_prefix |  The prefix of base training job name\. If `stop_training_on_fire` is true, the rule searches for SageMaker training jobs with this prefix in the same account\. If there is an inactivity found, the rule takes a `StopTrainingJob` action\. Note if there are multiple jobs found with same prefix, the rule skips termination\. It is important that the prefix is set unique per each training job\. **Optional** Valid values: String  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.stalled_training_rule(),
        rule_parameters={
                "threshold": "1800",
                "stop_training_on_fire": "True",
                "training_job_name_prefix": "<specify-training-base-job-name>"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="losses", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## TensorVariance<a name="tensor-variance"></a>

This rule detects if you have tensors with very high or low variances\. Very high or low variances in a tensor could lead to neuron saturation, which reduces the learning ability of the neural network\. Very high variance in tensors can also eventually lead to exploding tensors\. Use this rule to detect such issues early\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TensorVariance Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| max\_threshold |  The threshold for the upper bound of tensor variance\. **Optional** Valid values: Float Default value: None  | 
| min\_threshold |  The threshold for the lower bound of tensor variance\. **Optional** Valid values: Float Default value: None  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.tensor_variance(),
        rule_parameters={
                "collection_names": "weights",
                "max_threshold": "10",
                "min_threshold": "0.00001",
        },
        collections_to_save=[ 
            CollectionConfig(
                name="weights", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## UnchangedTensor<a name="unchanged-tensor"></a>

This rule detects whether a tensor is no longer changing across steps\. 

This rule runs the [numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html) method to check if the tensor isn't changing\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the UnchangedTensor Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patternsused to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| num\_steps |  The number of steps across which the rule checks to determine if the tensor has changed\.  This checks the last `num_steps` that are available\. They don't need to be consecutive\. If `num_steps` is 2, at step s it doesn't necessarily check for s\-1 and s\. If s\-1 isn't available, it checks the last available step along with s\. In that case, it checks the last available step with the current step\. **Optional** Valid values: Integer Default value: `3`  | 
| rtol |  The relative tolerance parameter to be passed to the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method\.  **Optional** Valid values: Float Default value: `1e-05`  | 
| atol |  The absolute tolerance parameter to be passed to the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method\. **Optional** Valid values: Float Default value: `1e-08`  | 
| equal\_nan |  Whether to compare NaNs as equal\. If `True`, NaNs in input array a are considered equal to NaNs in input array b in the output array\. This parameter is passed to the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method\. **Optional** Valid values: Boolean Default value: `False`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.unchanged_tensor(),
        rule_parameters={
                "collection_names": "losses",
                "tensor_regex": "",
                "num_steps": "3",
                "rtol": "1e-05",
                "atol": "1e-08",
                "equal_nan": "False"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="losses", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## CheckInputImages<a name="checkinput-mages"></a>

This rule checks if input images have been correctly normalized\. Specifically, it detects if the mean of the sample data differs by more than a threshold value from zero\. Many computer vision models require that input data has a zero mean and unit variance\.

This rule is applicable to deep learning applications\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the CheckInputImages Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| threshold\_mean |  A threshold that defines by how much mean of the input data can differ from 0\. **Optional** Valid values: Float Default value: `0.2`  | 
| threshold\_samples |  The number of images that have to be sampled before an error can be thrown\. If the value is too low, the estimation of the dataset mean will be inaccurate\. **Optional** Valid values: Integer Default value: `500`  | 
| regex |  The name of the input data tensor\. **Optional** Valid values: String Default value: `".*hybridsequential0_input_0"` \(the name of the input tensor for Apache MXNet models using HybridSequential\)  | 
| channel |  The position of the color channel in the input tensor shape array\.  **Optional** Valid values: Integer Default value: `1` \(for example, MXNet expects input data in the form of \(batch\_size, channel, height, width\)\)  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.check_input_images(),
        rule_parameters={
                "threshold_mean": "0.2",
                "threshold_samples": "500",
                "regex": ".*hybridsequential0_input_0",
                "channel": "1"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="custom_inputs_collection", 
                parameters={
                    "include_regex": ".*hybridsequential0_input_0"
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## NLPSequenceRatio<a name="nlp-sequence-ratio"></a>

This rule calculates the ratio of specific tokens given the rest of the input sequence that is useful for optimizing performance\. For example, you can calculate the percentage of padding end\-of\-sentence \(EOS\) tokens in your input sequence\. If the number of EOS tokens is too high, an alternate bucketing strategy should be performed\. You also can calculate the percentage of unknown tokens in your input sequence\. If the number of unknown words is too high, an alternate vocabulary could be used\.

This rule is applicable to deep learning applications\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the NLPSequenceRatio Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `".*embedding0_input_0"` \(assuming an embedding as the initial layer of the network\)  | 
| token\_values |  A string of a list of the numerical values of the tokens\. For example, "3, 0"\. **Optional** Valid values: Comma\-separated string of numerical values Default value: `0`  | 
| token\_thresholds\_percent |  A string of a list of thresholds \(in percentages\) that correspond to each of the `token_values`\. For example,"50\.0, 50\.0"\. **Optional** Valid values: Comma\-separated string of floats Default value: `"50"`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.nlp_sequence_ratio(),
        rule_parameters={
                "tensor_regex": ".*embedding0_input_0",
                "token_values": "0",
                "token_thresholds_percent": "50"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="custom_inputs_collection", 
                parameters={
                    "include_regex": ".*embedding0_input_0"
                } 
            )
        ]
    )
]
```

## Confusion<a name="confusion"></a>

This rule evaluates the goodness of a confusion matrix for a classification problem\.

It creates a matrix of size `category_no*category_no` and populates it with data coming from \(`labels`, `predictions`\) pairs\. For each \(`labels`, `predictions`\) pair, the count in `confusion[labels][predictions]` is incremented by 1\. When the matrix is fully populated, the ratio of data on\-diagonal values and off\-diagonal values are evaluated as follows:
+ For elements on the diagonal: `confusion[i][i]/sum_j(confusion[j][j])>=min_diag`
+ For elements off the diagonal: `confusion[j][i])/sum_j(confusion[j][i])<=max_off_diag`

This rule can be applied to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the Confusion Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| category\_no |  The number of categories\. **Optional** Valid values: Integer ≥2 Default value: `"None"`  | 
| labels |  The `labels` tensor collection or an 1\-d vector of true labels\.  **Optional** Valid values: String Default value: `"labels"`  | 
| predictions |  The `predictions` tensor collection or an 1\-d vector of estimated labels\.  **Optional** Valid values: String Default value: `"predictions"`  | 
| labels\_collection |  The rule inspects the tensors in this collection for `labels`\. **Optional** Valid values: String Default value: `"labels"`  | 
| predictions\_collection |  The rule inspects the tensors in this collection for `predictions`\. **Optional** Valid values: String Default value: `"predictions"`  | 
| min\_diag |  The minimum threshold for the ratio of data on the diagonal\. **Optional** Valid values: `0`≤float≤`1` Default value: `0.9`  | 
| max\_off\_diag |  The maximum threshold for the ratio of data off the diagonal\. **Optional** Valid values: `0`≤float≤`1` Default value: `0.1`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.confusion(),
        rule_parameters={
                "category_no": "10",
                "labels": "labels",
                "predictions": "predictions",
                "labels_collection": "labels",
                "predictions_collection": "predictions",
                "min_diag": "0.9",
                "max_off_diag": "0.1"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="labels",
                parameters={
                    "save_interval": "500"
                } 
            ),
            CollectionConfig(
                name="predictions",
                parameters={
                    "include_regex": "500"
                } 
            )
        ]
    )
]
```

**Note**  
This rule infers default values for the optional parameters if their values aren't specified\.

## FeatureImportanceOverweight<a name="feature_importance_overweight"></a>

This rule accumulates the weights of the n largest feature importance values per step and ensures that they do not exceed the threshold\. For example, you can set the threshold for the top 3 features to not hold more than 80 percent of the total weights of the model\.

This rule is valid only for the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the FeatureImportanceOverweight Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| threshold |  Defines the threshold for the proportion of the cumulative sum of the `n` largest features\. The number `n` is defined by the `nfeatures` parameter\. **Optional** Valid values: Float Default value: `0.8`  | 
| nfeatures |  The number of largest features\. **Optional** Valid values: Integer Default value: `3`  | 
| tensor\_regex |  Regular expression \(regex\) of tensor names the rule to analyze\. **Optional** Valid values: String Default value: `".*feature_importance/weight"`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.feature_importance_overweight(),
        rule_parameters={
                "threshold": "0.8",
                "nfeatures": "3",
                "tensor_regex": ".*feature_importance/weight"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="feature_importance", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```

## TreeDepth<a name="tree-depth"></a>

This rule measures the depth of trees in an XGBoost model\. XGBoost rejects splits if they do not improve loss\. This regularizes the training\. As a result, the tree might not grow as deep as defined by the `depth` parameter\.

This rule is valid only for the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TreeDepth Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The base trial training job name\. This parameter is automatically set to the current training job by Amazon SageMaker Debugger\.  **Required** Valid values: String  | 
| depth |  The depth of the tree\. The depth of the tree is obtained by computing the base 2 logarithm of the largest node ID\. **Optional** Valid values: Float Default value: `4`  | 

```
built_in_rules = [
    Rule.sagemaker(
        base_config=rule_configs.tree_depth(),
        rule_parameters={
                "depth": "4"
        },
        collections_to_save=[ 
            CollectionConfig(
                name="tree", 
                parameters={
                    "save_interval": "500"
                } 
            )
        ]
    )
]
```