# SageMaker integrations<a name="experiments-sm-integrations"></a>

Amazon SageMaker Experiments is integrated with a number of SageMaker features\. Certain SageMaker jobs automatically create experiments\. You can view and manage SageMaker Clarify bias reports or SageMaker Debugger output tensors for specific experiment runs directly in the Studio Experiments UI\.
+ [Automatic experiment creation](#experiments-sm-integrations-automatic)
+ [Bias and explainability reports](#experiments-sm-integrations-reports)
+ [Debugging](#experiments-sm-integrations-debugs)

## Automatic experiment creation<a name="experiments-sm-integrations-automatic"></a>

Amazon SageMaker automatically creates experiments when running Autopilot jobs, hyperparameter optimization \(HPO\) jobs, or Pipeline executions\. You can view these experiments in the Studio Experiments UI\.

### Autopilot<a name="experiments-sm-integrations-autopilot"></a>

Amazon SageMaker Experiments is integrated with Amazon SageMaker Autopilot\. When you perform an Autopilot job, SageMaker Experiments creates an experiment for that job as well as runs for each of the different combinations of the available run components, parameters, and artifacts\. You can find these runs in the SageMaker Experiments UI by filtering for the run type **Autopilot**\. For more information, see [Automate model development with Amazon SageMaker Autopilot](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development.html)\.

### HPO<a name="experiments-sm-integrations-hpo"></a>

Amazon SageMaker Experiments is integrated with HPO jobs\. An HPO job automatically creates Amazon SageMaker experiments, runs, and components for each training job that it completes\. You can find these runs in the SageMaker Experiments UI by filtering for the run type **HPO**\. For more information, see [Tune Multiple Algorithms with Hyperparameter Optimization to Find the Best Model](https://docs.aws.amazon.com/sagemaker/latest/dg/multiple-algorithm-hpo.html)\.

### Pipelines<a name="experiments-sm-integrations-pipelines"></a>

Amazon SageMaker Model Building Pipelines is closely integrated with Amazon SageMaker Experiments\. By default, when SageMaker Pipelines creates and executes a pipeline, experiments, runs, and components are created if they do not already exist\. You can find these runs in the SageMaker Experiments UI by filtering for the run type **Pipelines**\. For more information, see [Amazon SageMaker Experiments Integration](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-experiments.html)\.

## Bias and explainability reports<a name="experiments-sm-integrations-reports"></a>

Manage SageMaker Clarify bias and explainability reports for experiment runs directly through Studio\. To view reports, find and select the name of the experiment run of your choice in Studio\. Choose **Bias reports** to see any Clarify bias reports associated with the experiment run\.

![\[A SageMaker Clarify bias report for an experiment run in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-bias-report.png)

Choose **Explanations** to see any Clarify explainability reports associated with the experiment run\.

![\[A SageMaker Clarify bias report for an experiment run in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-explainability-report.png)

You can generate pre\-training or post\-training bias reports that analyze bias in datasets or model predictions using labels and bias metrics with SageMaker Clarify\. You can also use SageMaker Clarify to generate explainability reports that document model behavior for global or local data samples\. For more information, see [Amazon SageMaker Clarify Bias Detection and Model Explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html)\.

## Debugging<a name="experiments-sm-integrations-debugs"></a>

You can debug model training progress with Amazon SageMaker Debugger and view debug output tensors in the Studio Experiments UI\. Choose the name of the run associated with the Debugger report and choose **Debugs**\.

![\[Debugger overview page for an experiment run in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-debugs.png)

Then, choose the training job name to view the associated Amazon SageMaker Debugger dashboard\.

![\[Debugger insights dashboard example in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-debugger-report.png)

For more information, see [Debug Training Jobs Using Amazon SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-on-studio.html)\.