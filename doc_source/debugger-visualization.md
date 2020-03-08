# Amazon SageMaker Studio Visualizations of Model Analysis Results<a name="debugger-visualization"></a>

Amazon SageMaker Studio provides visualizations to interpret tensor outputs from model analysis\. 

**Loss curves while training is in progress**

The following screenshot shows visualizations of loss curves for training\. The training is in progress\.

![\[An image containing training trial visualizations.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger-visualize-loss-curves-rules.png)

**Analyzing training jobs \- comparing loss curves across multiple jobs**

Amazon SageMaker Studio allows simple comparison across multiple jobs \- in this case the loss\. This helps identify the best\-performing training jobs\.

![\[Comparing loss curves..\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/degubber-analyze-training-loss-curves.png)

**Rules triggering \+ logs from Debugger Jobs**

When rules are triggered for anomalous conditions, Amazon SageMaker Studio presents logs for the failing rule, allowing easy analysis of the causes of the condition\.

![\[Rules triggered.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger-rules-triggered.png)