# Best Practices for Amazon SageMaker Debugger<a name="debugger-best-practices"></a>

Use the following guidelines when you run training jobs with Debugger\. 

**Topics**
+ [Choose a Machine Learning Framework](#debugger-how-it-works-1)
+ [Use Studio Debugger Insights Dashboard](#debugger-how-it-works-studio-dashboard)
+ [Download Debugger Reports and Gain More Insights](#debugger-how-it-works-4)
+ [Capture Data from Your Training Job and Save Data to Amazon S3](#debugger-how-it-works-2)
+ [Analyze the Data with a Fleet of Debugger Built\-in Rules](#debugger-how-it-works-3)
+ [Take Actions Based on the Built\-in Rule Status](#debugger-how-it-works-action-on-rules)
+ [Dive Deep into the Data Using the SMDebug Client Library](#debugger-how-it-works-5)
+ [Monitoring System Utilization and Detect Bottlenecks](#w1842aac27c16c44c21)
+ [Profiling Framework Operations](#w1842aac27c16c44c23)
+ [Debugging Model Parameters](#w1842aac27c16c44c25)

## Choose a Machine Learning Framework<a name="debugger-how-it-works-1"></a>

You can choose a machine learning framework and use SageMaker pre\-built training containers or your own containers\. Use Debugger to detect training and performance issues, and analyze training progress of your training job in SageMaker\. SageMaker provides you options to use pre\-built containers that are prepared for a number of machine learning framework environments to train your model on Amazon EC2\. Any training job can be adapted to run in AWS Deep Learning Containers, SageMaker training containers, and custom containers\. To learn more, see [Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration.md) and [Use Debugger with Custom Training Containers](debugger-bring-your-own-container.md)\.

## Use Studio Debugger Insights Dashboard<a name="debugger-how-it-works-studio-dashboard"></a>

With Studio Debugger insights dashboard, you are in control of your training jobs\. Use the Studio Debugger dashboards to keep your model performance on Amazon EC2 instances in control and optimized\. For any SageMaker training jobs running on Amazon EC2 instance, Debugger monitors resource utilization and basic model output data \(loss and accuracy values\)\. Through the Studio Debugger dashboards, gain insights into your training jobs and improve your model training performance\. To learn more, see [SageMaker Debugger on Studio](debugger-on-studio.md)\.

## Download Debugger Reports and Gain More Insights<a name="debugger-how-it-works-4"></a>

You can view aggregated results and gain insights in Debugger reports\. Debugger aggregates training and profiling results collected from the built\-in rule analysis into a report per training job\. You can find more detailed information about your training results through the Debugger reports\. To learn more, see [SageMaker Debugger Interactive Reports](debugger-report.md)\.

## Capture Data from Your Training Job and Save Data to Amazon S3<a name="debugger-how-it-works-2"></a>

You can use a Debugger hook to save output tensors\. After you choose a container and a framework that fit your training script, use a Debugger hook to configure which tensors to save and to which directory to save them, such as a Amazon S3 bucket\. A Debugger hook helps you to build the configuration and to keep it in your account to use in subsequent analyses, where it is secured for use with the most privacy\-sensitive applications\. To learn more, see [Configure Debugger Hook to Save Tensors](debugger-configure-hook.md)\.

## Analyze the Data with a Fleet of Debugger Built\-in Rules<a name="debugger-how-it-works-3"></a>

You can use Debugger built\-in rules to inspect tensors in parallel with a training job\. To analyze the training performance data, Debugger provides built\-in rules that watch for abnormal training process behaviors\. For example, a Debugger rule detects issues when the training process suffers from system bottleneck issues or training issues, such as vanishing gradients, exploding tensors, overfitting, or overtraining\. If necessary, you can also build customized rules by creating a rule definition with your own criteria to define a training issue\. To learn more about the Debugger rules, see [Configure Debugger Built\-in Rules](use-debugger-built-in-rules.md) for detailed instructions of using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. For a full list of the Debugger built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\. If you want to create a custom rule, see [Create Debugger Custom Rules for Training Job Analysis](debugger-custom-rules.md)\.

## Take Actions Based on the Built\-in Rule Status<a name="debugger-how-it-works-action-on-rules"></a>

You can use Debugger with Amazon CloudWatch Events and AWS Lambda\. You can automate actions based on the rule status, such as stopping training jobs early and setting up notifications through email or text\. When the Debugger rules detect problems and triggers an `"IssuesFound"` evaluation status, CloudWatch Events detects the rule status changes and invokes the Lambda function to take actions\. To configure automated actions to your training issues, see [Create Actions on Rules Using Amazon CloudWatch and AWS Lambda](debugger-cloudwatch-lambda.md)\.

## Dive Deep into the Data Using the SMDebug Client Library<a name="debugger-how-it-works-5"></a>

You can use the SMDebug tools to access and analyze training data collected by Debugger\. The `TrainingJob` and `create_trial` classes load the metrics and tensors saved by Debugger\. These classes provide extended class methods to analyze the data in real time or after the training has finished\. The SMDebug library also provides visualization tools: merge timelines of framework metrics to aggregate different profiling, line charts and heatmap to track the system utilization, and histograms to find step duration outliers\. To learn more about the SMDebug library tools, see [Analyze Data Using the SMDebug Client Library](debugger-analyze-data.md)\.

## Monitoring System Utilization and Detect Bottlenecks<a name="w1842aac27c16c44c21"></a>

With Amazon SageMaker Debugger monitoring, you can measure hardware system resource utilization of Amazon EC2 instances\. Monitoring is available for any SageMaker training job constructed with the SageMaker framework estimators \(TensorFlow, PyTorch, and MXNet\) and the generic SageMaker estimator \(SageMaker built\-in algorithms and your own custom containers\)\. Debugger built\-in rules for monitoring detect system bottleneck issues and notify you when they detect the bottleneck issues\.

To learn how to enable Debugger system monitoring, see [Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration.md) and then [Configure Debugger Monitoring Hardware System Resource Utilization](debugger-configure-system-monitoring.md)\.

For a full list of available built\-in rules for monitoring, see [Debugger Built\-in Rules for Monitoring Hardware System Resource Utilization \(System Metrics\)](debugger-built-in-rules.md#built-in-rules-monitoring)\.

## Profiling Framework Operations<a name="w1842aac27c16c44c23"></a>

With Amazon SageMaker Debugger profiling you can profile deep learning frameworks operations\. You can profile your model training with the SageMaker TensorFlow training containers, the SageMaker PyTorch framework containers, and your own training containers\. Using the profiling feature of Debugger, you can drill down into the Python operators and functions that are executed to perform the training job\. Debugger supports detailed profiling, Python profiling, data loader profiling, and Horovod distributed training profiling\. You can merge the profiled timelines to correlate with the system bottlenecks\. Debugger built\-in rules for profiling watch framework operation related issues, including excessive training initialization time due to data downloading before training starts and step duration outliers in training loops\. 

To learn how to configure Debugger for framework profiling, see [Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration.md) and then [Configure Debugger Framework Profiling](debugger-configure-framework-profiling.md)\.

For a complete list of available built\-in rules for profiling, see [Debugger Built\-in Rules for Profiling Framework Metrics](debugger-built-in-rules.md#built-in-rules-profiling)\.

## Debugging Model Parameters<a name="w1842aac27c16c44c25"></a>

Debugging is available for deep learning frameworks using AWS Deep Learning Containers and the SageMaker training containers\. For fully supported framework versions \(see the versions at [Supported Frameworks and Algorithms](debugger-supported-frameworks.md)\), Debugger automatically registers hooks to collect output tensors, and you can directly run your training script\. For the versions with one asterisk sign, you need to manually register the hooks to collect tensors\. Debugger provides preconfigured tensor collections with generalized names that you can utilize across the different frameworks\. If you want to customize output tensor configuration, you can also use the CollectionConfig and DebuggerHookConfig API operations and the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to configure your own tensor collections\. Debugger built\-in rules for debugging analyze the output tensors and identifies model optimization problems that blocks your model from minimizing the loss function\. For example, the rules identify overfitting, overtraining, loss not decreasing, exploding tensors, and vanishing gradients\.

To learn how to configure Debugger for debugging output tensors, see [Configure Debugger Using Amazon SageMaker Python SDK](debugger-configuration.md) and then [Configure Debugger Hook to Save Tensors](debugger-configure-hook.md)\.

For a full list of available built\-in rules for debugging, see [Debugger Built\-in Rules for Debugging Model Training Data \(Output Tensors\)](debugger-built-in-rules.md#built-in-rules-debugging)\.