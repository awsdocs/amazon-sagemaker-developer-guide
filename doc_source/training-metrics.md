# Monitor and Analyze Training Jobs Using Metrics<a name="training-metrics"></a>

Monitor and analyze training jobs by viewing metrics that the training jobs send to Amazon CloudWatch\. An Amazon SageMaker training job is an iterative process of teaching a model to make predictions by presenting examples from a training dataset\. Typically, a training algorithm computes several metrics, such as training error and prediction accuracy\. These metrics help diagnose whether the model is learning well and will generalize well for making predictions on unseen data\. The training algorithm writes the values of these metrics to logs, which Amazon SageMaker monitors and sends to CloudWatch in real time\. To analyze the performance of your training job, you can view graphs of these metrics in CloudWatch\. When a training job has completed, you can also get a list of the metric values that it computes in its final iteration by calling the [DescribeTrainingJob](API_DescribeTrainingJob.md) operation\.

**Topics**
+ [Training Metrics Sample Notebooks](#training-metrics-sample-notebooks)
+ [Define Training Metrics](#define-train-metrics)
+ [Monitor Training Job Metrics in CloudWatch](#view-train-metrics-cw)
+ [Example: View a Training and Validation Curve](#train-valid-curve)

## Training Metrics Sample Notebooks<a name="training-metrics-sample-notebooks"></a>

The following sample notebooks show how to view and plot training metrics\.
+ [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/introduction\_to\_amazon\_algorithms/object2vec\_sentence\_similarity/object2vec\_sentence\_similarity\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object2vec_sentence_similarity/object2vec_sentence_similarity.ipynb)

  [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/introduction\_to\_amazon\_algorithms/xgboost\_abalone/xgboost\_abalone\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_abalone.ipynb)

For instructions how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Using Example Notebooks](howitworks-nbexamples.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the Amazon SageMaker samples\. The example notebooks that show how to use training metrics are located in the **Introduction to Amazon algorithms** section, and named `object2vec_sentence_similarity.ipynb` and `xgboost_abalone.ipynb`\. To open a notebook, click on its **Use** tab and select **Create copy**\.

## Define Training Metrics<a name="define-train-metrics"></a>

Amazon SageMaker automatically parses the logs for metrics that built\-in algorithms emit and sends those metrics to CloudWatch\. If you use your own algorithm, when you configure the training job, you have to specify the metrics that you want Amazon SageMaker to send to CloudWatch\. You do this by specifying the name of the metrics that you want to send and the regular expressions that Amazon SageMaker uses to parse the logs that your algorithm emits to find those metrics\.

You can specify the metrics that you want to track with the AWS Management Console, the Amazon SageMaker Python SDK \([https://github\.com/aws/sagemaker\-python\-sdk](https://github.com/aws/sagemaker-python-sdk)\), or the low\-level Amazon SageMaker API\.

**Topics**
+ [Define Training Metrics \(Low\-level Amazon SageMaker API\)](#define-train-metrics-api)
+ [Define Training Metrics \(Amazon SageMaker Python SDK\)](#define-train-metrics-sdk)
+ [Define Metrics \(Console\)](#define-train-metrics-console)
+ [Define Regular Expressions for Metrics](#define-train-metric-regex)

### Define Training Metrics \(Low\-level Amazon SageMaker API\)<a name="define-train-metrics-api"></a>

Define the metrics that you want to send to CloudWatch by specifying a list of metric names and regular expressions in the `MetricDefinitions` field of the [AlgorithmSpecification](API_AlgorithmSpecification.md) input parameter that you pass to the [CreateTrainingJob](API_CreateTrainingJob.md) operation\. For example, if your algorithm emits metrics for training error and validation error, and you want to monitor both of those metrics in CloudWatch, your `AlgorithmSpecification` would look like the following:

```
"AlgorithmSpecification": {
        "TrainingImage": ContainerName,
        "TrainingInputMode": "File",
        "MetricDefinitions" : [
            {
            "Name": "train:error",
            "Regex": ".*\\[[0-9]+\\]#011train-error:(\\S+).*"
        },
             {
            "Name": "validation:error",
            "Regex": ".*\\[[0-9]+\\]#011validation-error:(\\S+).*"
        }
        
    ]}
```

For more information about defining and running a training job by using the low\-level Amazon SageMaker API, see [Step 3\.3\.2: Create a Training Job](ex1-train-model-create-training-job.md)\.

### Define Training Metrics \(Amazon SageMaker Python SDK\)<a name="define-train-metrics-sdk"></a>

Define the metrics that you want to send to CloudWatch by specifying a list of metric names and regular expressions as the `metric_definitions` argument when you initialize an `Estimator` object\. For example, if your algorithm emits metrics for training error and validation error, and you want to monitor both of those metrics in CloudWatch, your `Estimator` initialization would look like the following:

```
estimator =
                Estimator(image_name=ImageName,
                role='SageMakerRole', train_instance_count=1,
                train_instance_type='ml.c4.xlarge',
                train_instance_type='ml.c4.xlarge',
                k=10,
                sagemaker_session=sagemaker_session,
                metric_definitions=[
                   {'Name': 'train:error', 'Regex': '.*\\[[0-9]+\\]#011train-error:(\\S+).*'},
                   {'Name': 'validation:error, 'Regex': '.*\\[[0-9]+\\]#011validation-error:(\\S+).*'
                ]
            )
```

For more information about training by using Amazon SageMaker Python SDK estimators, see [https://github\.com/aws/sagemaker\-python\-sdk\#sagemaker\-python\-sdk\-overview](https://github.com/aws/sagemaker-python-sdk#sagemaker-python-sdk-overview)``\.

### Define Metrics \(Console\)<a name="define-train-metrics-console"></a>

You can define metrics for a custom algorithm in the console when you create a training job by providing the name and regular expression \(regex\) under **Metrics**\.

### Define Regular Expressions for Metrics<a name="define-train-metric-regex"></a>

To find a metric, Amazon SageMaker searches the logs that your algorithm emits and finds logs that match the regular expression that you specify for that metric\. If you are using your own algorithm, make sure that the algorithm writes the metrics you want to capture to logs, and that you define a regular expression that accurately searches the logs to capture the values of the metrics that you want to send to CloudWatch metrics\.

## Monitor Training Job Metrics in CloudWatch<a name="view-train-metrics-cw"></a>

You can monitor the metrics that a training job emits in real time by using the CloudWatch console\.

**To monitor training job metrics in CloudWatch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose **/aws/sagemaker/TrainingJobs**\.

1. Choose **TrainingJobName**\.

1. On the **All metrics** tab, choose the names of the training metrics that you want to monitor\.

1. On the **Graphed metrics** tab, configure the graph options\. For more information about using CloudWatch graphs, see [Graph Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_metrics.html) in the *Amazon CloudWatch User Guide*\.

## Example: View a Training and Validation Curve<a name="train-valid-curve"></a>

A common way to analyze the performance of a training job is to plot a training curve against a validation curve\. Typically, you split the data that you train your model on into training and validation datasets\. You use the training set to train the model parameters that are used to make predictions on the training dataset\. Then you test how well it makes predictions by calculating predictions for the validation set\. Viewing a graph that shows the accuracy for both the training and validation set over time can help you to train your model\. For example, if the training accuracy continues to increase over time, but, at some point, the validation accuracy starts to decrease, you are likely overfitting your model\. To address this, you might consider making adjustments to your model, such as increasing *[regularization](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#regularization)*\.

For this example, use the **Image\-classification\-full\-training** example that is in the **Example notebooks** section of your Amazon SageMaker notebook instance\. If you do not already have a Amazon SageMaker notebook instance, create one by following the instructions at [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\. You can also see this example notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/introduction\_to\_amazon\_algorithms/imageclassification\_caltech/Image\-classification\-fulltraining\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-fulltraining.ipynb)\. You also need an Amazon S3 bucket to store the training data and for the model output\. If you have not already created a bucket to use for Amazon SageMaker, create one by following the instructions at [Step 1\.2: Create an S3 Bucket](gs-config-permissions.md)\.

**To plot a training and validation error curve:**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Notebooks**, and then choose **Notebook instances**\.

1. Choose the notebook instance you want to use, and then choose **Open**\.

1. On the dashboard of your notebook instance, choose **SageMaker Examples**\.

1. Expand the **Introduction to Amazon Algorithms** section, and then next to **Image\-classification\-full\-training\.ipynb**, choose **Use**\.

1. Choose **Create copy**\. Amazon SageMaker creates an editable copy of the **Image\-classification\-full\-training\.ipynb** notebook in your notebook instance\.

1. In the first code cell of the notebook, replace *<<bucket\-name>>* with the name of your S3 bucket\.

1. Run all of the cells in the notebook up to the **Deploy** section\. You don't need to deploy an endpoint or get inference for this example\.

1. After the training job starts, open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose **/aws/sagemaker/TrainingJobs**\.

1. Choose **TrainingJobName**\.

1. On the **All metrics** tab, select the metric names **train:accuracy** and **validation:accuracy** for the training job you created in the notebook\.

1. On the graph, choose a region where you see the values of the metric to zoom in on that section\. You should see something like the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/train-valid-acc.png)