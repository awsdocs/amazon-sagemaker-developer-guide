# Track the Lineage of a SageMaker ML Pipeline<a name="pipelines-lineage-tracking"></a>

In this tutorial, you use Amazon SageMaker Studio to track the lineage of an Amazon SageMaker ML Pipeline\.

The pipeline was created by the [Orchestrating Jobs with Amazon SageMaker Model Building Pipelines](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-pipelines/tabular/abalone_build_train_deploy/sagemaker-pipelines-preprocess-train-evaluate-batch-transform.html) notebook in the [Amazon SageMaker example GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\. For detailed information on how the pipeline was created, see [Define a Pipeline](define-pipeline.md)\.

Lineage tracking in Studio is centered around a directed acyclic graph \(DAG\)\. The DAG represents the steps in a pipeline\. From the DAG you can track the lineage from any step to any other step\. The following diagram displays the steps in the pipeline\. These steps appear as a DAG in Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-steps.png)

**Prerequisites**
+ Access to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.
+ Familiarity with the SageMaker Studio user interface\. For more information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.
+ \(Recommended\) A completed run of the example notebook\.

**To track the lineage of a pipeline**

1. Sign in to SageMaker Studio\.

1. In the left sidebar of Studio, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. In the menu, select **Pipelines**\.

1. Use the **Search** box to filter the pipelines list\. To view all available columns, drag the right border of the pane to the right\. For more information, see [Search Experiments Using Amazon SageMaker Studio](experiments-search-studio.md)\.

   The following screenshot shows the list filtered by a name that starts with "aba" and that was created on 12/5/20\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-search.png)

1. Double\-click the `AbalonePipeline` pipeline to view the execution list and other details about the pipeline\. The following screenshot shows the **TABLE PROPERTIES** pane open where you can choose which properties to view\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-executions.png)

1. Choose the **Settings** tab and then choose **Download pipeline definition file**\. You can view the file to see how the pipeline graph was defined\.

1. On the **Execution** tab, double\-click the first row in the execution list to view its execution graph and other details about the execution\. Note that the graph matches the diagram displayed at the beginning of the tutorial\.

   You can drag the graph around \(select an area not on the graph itself\) or use the resizing icons on the lower\-left side of the graph\. The inset on the lower\-right side of the graph displays your location in the graph\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-execution-graph.png)

1. On the **Graph** tab, choose the `AbaloneProcess` step to view details about the step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-process-step.png)

1. Find the Amazon S3 paths to the training, validation, and test datasets in the **Output** tab, under **Files**\.
**Note**  
To get the full paths, right\-click the path and then choose **Copy cell contents**\.

   ```
   s3://sagemaker-eu-west-1-acct-id/sklearn-abalone-process-2020-12-05-17-28-28-509/output/train
   s3://sagemaker-eu-west-1-acct-id/sklearn-abalone-process-2020-12-05-17-28-28-509/output/validation
   s3://sagemaker-eu-west-1-acct-id/sklearn-abalone-process-2020-12-05-17-28-28-509/output/test
   ```

1. Choose the `AbaloneTrain` step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-train-step.png)

1. Find the Amazon S3 path to the model artifact in the **Output** tab, under **Files**:

   ```
   s3://sagemaker-eu-west-1-acct-id/AbaloneTrain/pipelines-6locnsqz4bfu-AbaloneTrain-NtfEpI0Ahu/output/model.tar.gz
   ```

1. Choose the `AbaloneRegisterModel` step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-tutorial-register-model-step.png)

1. Find the ARN of the model package in the **Output** tab, under **Files**:

   ```
   arn:aws:sagemaker:eu-west-1:acct-id:model-package/abalonemodelpackagegroupname/2
   ```