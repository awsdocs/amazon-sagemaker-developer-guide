# Debugger in Studio Experiments<a name="debugger-on-studio-experiments"></a>

In this section, you learn how to use the Debugger in Studio Experiments\. You can select any training jobs from the Experiment trial list to see the model output data graphs, such as accuracy and loss curves, debugging built\-in rule status, and Debugger configuration information for debugging\.

## Visualize Tensors Using SageMaker Debugger and Studio<a name="debugger-visualization-studio"></a>

SageMaker Studio provides visualizations to interpret tensor outputs that are captured by Debugger\. 

### Loss Curves While Training Is in Progress<a name="loss-curves-during-training"></a>

The following screenshot shows visualizations of loss curves for training\. The training is in progress\.

![\[An image containing training trial visualizations\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-visualize-loss-curves-rules.png)

### Analyzing Training Jobs: Comparing Loss Curves Across Multiple Jobs<a name="loss-curves-across-multiple-jobs"></a>

SageMaker Studio enables you to compare across multiple jobs \(in this case, the loss\)\. This helps you identify the best\-performing training jobs\.

![\[An image showing a comparison of loss curves\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/degubber-analyze-training-loss-curves.png)

### Rules Triggering and Logs from Jobs<a name="rules-triggering-and-logs"></a>

When rules are triggered for anomalous conditions, SageMaker Studio presents logs for the failing rule, which enables you to analyze the causes of the condition\.

![\[An image showing rules triggered\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-rules-triggered.png)