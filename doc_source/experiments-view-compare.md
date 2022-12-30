# View, search, and compare experiment runs<a name="experiments-view-compare"></a>

An Amazon SageMaker *experiment* consists of multiple *run groups* with a related objective\. A run group consists of one or more *runs*, such as a data preprocessing job and a training job\.

You use the experiments browser to display a list of these entities\. You can filter the list by entity name, type, and tags\. For an overview of the Studio user interface, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [View experiments and runs](#experiments-view)
+ [Compare and analyze runs](#experiments-compare)

## View experiments and runs<a name="experiments-view"></a>

Amazon SageMaker Studio provides an experiments browser that you can use to view lists of experiments and runs\. You can choose one of these entities to view detailed information about the entity or choose multiple entities for comparison\.

**To view experiments and runs**

1. To view the experiment in Studio, in the left sidebar, choose **Experiments**\.

   Select the name of the experiment to view all associated runs\. You can search experiments by typing directly into the **Search** bar or filtering for experiment type\. You can also choose which columns to display in your experiment or run list\.

   It might take a moment for the list to refresh and display a new experiment or experiment run\. You can click **Refresh** to update the page\. Your experiment list should look similar to the following:  
![\[A list of experiments in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-overview.png)

1. In the experiments list, double\-click an experiment to display a list of the runs in the experiment\.  
![\[A list of experiment runs in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-runs-overview.png)

1. Double\-click a run to display information about a specific run\.

   In the **Overview** pane, choose any of the following headings to see available information about each run:
   + **Metrics** – Metrics that are logged during a run\.
   + **Charts** – Build your own charts to compare runs\.
   + **Output artifacts** – Any resulting artifacts of the experiment run and the artifact locations in Amazon S3\.
   + **Bias reports** – Pe\-training or post\-training bias reports generated using Clarify\.
   + ** Explainability**– Explainability reports generated using Clarify\.
   + **Debugs** – A list of debugger rules and any issues found\.

## Compare and analyze runs<a name="experiments-compare"></a>

To analyze experiment runs, select the experiment of your choice in the Amazon SageMaker Studio Experiments UI and then select the runs that you want to compare\. You must select between 1 and 20 runs\. After you have your runs selected, choose **Analyze** in the upper right\-hand corner\.

**To compare experiment runs:**

1. After navigating to the experiment of your choice, select all the runs that you want to compare\. You must choose more than 1 and less than 20 runs to analyze\.

1. Choose **Analyze** in the upper right\-hand corner\.

1. Visualize the comparative metrics of multiple experiment runs in a histogram, line chart, scatter plot, or bar chart\. To add a chart, choose **Add Chart**, select values for your chart axes, and choose **Create**\.

![\[A selection of experiment runs to analyze in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-runs-compare.png)

You can update, download, or delete existing charts\.

![\[A line graph comparing the validation loss of three different experiment runs in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-runs-analyze.png)

### Automatic logging<a name="experiments-compare-automatic-logging"></a>

Automatic logging and visualization is available for classification models\. You can automatically log a confusion matrix, receiver operating characteristics, or precision and recall graphs\. 

Log and visualize metrics with the following Python SDK methods:
+ `log_confusion_matrix`: Records a confusion matrix artifact that you can view in the **Charts** section of the **Run Overview** in Studio\.
+ `log_roc_curve`: Records a receiver operating characteristic artifact that you can view in the **Charts** section of the **Run Overview** in Studio\.
+ `log_precision_recall`: Records a precision recall graph that you can view in the **Charts** section of the **Run Overview** in Studio\.

An automatically logged precision recall record creates a chart similar to the following:

![\[A precision recall chart for an experiment run in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-charts-precision-recall.png)