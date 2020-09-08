# Monitor and Analyze Training Jobs Using Metrics<a name="training-metrics"></a>

A Amazon SageMaker training job is an iterative process that teaches a model to make predictions by presenting examples from a training dataset\. Typically, a training algorithm computes several metrics, such as training error and prediction accuracy\. These metrics help diagnose whether the model is learning well and will generalize well for making predictions on unseen data\. The training algorithm writes the values of these metrics to logs, which SageMaker monitors and sends to Amazon CloudWatch in real time\. To analyze the performance of your training job, you can view graphs of these metrics in CloudWatch\. When a training job has completed, you can also get a list of the metric values that it computes in its final iteration by calling the [ `DescribeTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) operation\.

**Topics**
+ [Training Metrics Sample Notebooks](#training-metrics-sample-notebooks)
+ [Defining Training Metrics](#define-train-metrics)
+ [Monitoring Training Job Metrics \( Console\)](#view-train-metrics-cw)
+ [Monitoring Training Job Metrics \(SageMaker Console\)](#view-train-metrics-sm)
+ [Example: Viewing a Training and Validation Curve](#train-valid-curve)

## Training Metrics Sample Notebooks<a name="training-metrics-sample-notebooks"></a>

The following sample notebooks show how to view and plot training metrics:
+ [ An Introduction to the Amazon SageMaker ObjectToVec Model for Sequence\-to\-sequence Embedding \(object2vec\_sentence\_similarity\.ipynb\)](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object2vec_sentence_similarity/object2vec_sentence_similarity.ipynb)
+ [Regression with the Amazon SageMaker XGBoost Algorithm \(xgboost\_abalone\.ipynb\)](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_abalone.ipynb)

For instructions how to create and access Jupyter notebook instances that you can use to run the examples in SageMaker, see [Example Notebooks](howitworks-nbexamples.md)\. To see a list of all the SageMaker samples, after creating and opening a notebook instance, choose the **SageMaker Examples** tab\. To access the example notebooks that show how to use training metrics, `object2vec_sentence_similarity.ipynb` and ` xgboost_abalone.ipynb`\., from the **Introduction to Amazon algorithms** section\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.

## Defining Training Metrics<a name="define-train-metrics"></a>

SageMaker automatically parses the logs for metrics that built\-in algorithms emit and sends those metrics to CloudWatch\. If you want SageMaker to parse logs from a custom algorithm and send metrics that the algorithm emits to CloudWatch, you have to specify the metrics that you want SageMaker to send to CloudWatch when you configure the training job\. You specify the name of the metrics that you want to send and the regular expressions that SageMaker uses to parse the logs that your algorithm emits to find those metrics\.

You can specify the metrics that you want to track with the SageMaker console;, the SageMaker Python SDK \([https://github\.com/aws/sagemaker\-python\-sdk](https://github.com/aws/sagemaker-python-sdk)\), or the low\-level SageMaker API\.

**Topics**
+ [Defining Regular Expressions for Metrics](#define-train-metric-regex)
+ [Defining Training Metrics \(Low\-level SageMaker API\)](#define-train-metrics-api)
+ [Defining Training Metrics \(SageMaker Python SDK\)](#define-train-metrics-sdk)
+ [Define Training Metrics \(Console\)](#define-train-metrics-console)

### Defining Regular Expressions for Metrics<a name="define-train-metric-regex"></a>

To find a metric, SageMaker searches the logs that your algorithm emits and finds logs that match the regular expression that you specify for that metric\. If you are using your own algorithm, do the following:
+ Make sure that the algorithm writes the metrics that you want to capture to logs
+ Define a regular expression that accurately searches the logs to capture the values of the metrics that you want to send to CloudWatch metrics\.

For example, suppose your algorithm emits metrics for training error and validation error by writing logs similar to the following to `stdout ` or `stderr`:

```
Train_error=0.138318;  Valid_error = 0.324557;
```

If you want to monitor both of those metrics in CloudWatch, your `AlgorithmSpecification` would look like the following:

```
"AlgorithmSpecification": {
        "TrainingImage": ContainerName,
        "TrainingInputMode": "File",
        "MetricDefinitions" : [
            {
            "Name": "train:error",
            "Regex": "Train_error=(.*?);"
        },
             {
            "Name": "validation:error",
            "Regex": "Valid_error=(.*?);"
        }
        
    ]}
```

In the regex for the `train:error` metric defined above, the first part of the regex finds the exact text "Train\_error=", and the expression `(.*?);` captures zero or more of any character until the first semicolon character\. In this expression, the parenthesis tell the regex to capture what is inside them, `.` means any character, `*` means zero or more, and `?` means capture only until the first instance of the `;` character\.

### Defining Training Metrics \(Low\-level SageMaker API\)<a name="define-train-metrics-api"></a>

Define the metrics that you want to send to CloudWatch by specifying a list of metric names and regular expressions in the `MetricDefinitions` field of the [ `AlgorithmSpecification`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html) input parameter that you pass to the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) operation\. For example, if you want to monitor both the `train:error` and `validation:error` metrics in CloudWatch, your `AlgorithmSpecification` would look like the following:

```
"AlgorithmSpecification": {
        "TrainingImage": ContainerName,
        "TrainingInputMode": "File",
        "MetricDefinitions" : [
            {
            "Name": "train:error",
            "Regex": "Train_error=(.*?);"
        },
             {
            "Name": "validation:error",
            "Regex": "Valid_error=(.*?);"
        }
        
    ]}
```

For more information about defining and running a training job by using the low\-level SageMaker API, see [Create and Run a Training Job \(AWS SDK for Python \(Boto3\)\)](ex1-train-model.md#ex1-train-model-create-training-job)\.

### Defining Training Metrics \(SageMaker Python SDK\)<a name="define-train-metrics-sdk"></a>

Define the metrics that you want to send to CloudWatch by specifying a list of metric names and regular expressions as the `metric_definitions` argument when you initialize an `Estimator` object\. For example, if you want to monitor both the `train:error` and `validation:error` metrics in CloudWatch, your `Estimator` initialization would look like the following:

```
estimator =
                Estimator(image_name=ImageName,
                role='SageMakerRole', train_instance_count=1,
                train_instance_type='ml.c4.xlarge',
                train_instance_type='ml.c4.xlarge',
                k=10,
                sagemaker_session=sagemaker_session,
                metric_definitions=[
                   {'Name': 'train:error', 'Regex': 'Train_error=(.*?);'},
                   {'Name': 'validation:error', 'Regex': 'Valid_error=(.*?);'}
                ]
            )
```

For more information about training by using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) estimators, see [https://github\.com/aws/sagemaker\-python\-sdk\#sagemaker\-python\-sdk\-overview](https://github.com/aws/sagemaker-python-sdk#sagemaker-python-sdk-overview)\.

### Define Training Metrics \(Console\)<a name="define-train-metrics-console"></a>

You can define metrics for a custom algorithm in the console when you create a training job by providing the name and regular expression \(regex\) for **Metrics**\.

For example, if you want to monitor both the `train:error` and `validation:error` metrics in CloudWatch, your metric definitions would look like the following:

```
[
            {
            "Name": "train:error",
            "Regex": "Train_error=(.*?);"
        },
             {
            "Name": "validation:error",
            "Regex": "Valid_error=(.*?);"
        }
        
    ]
```

## Monitoring Training Job Metrics \( Console\)<a name="view-train-metrics-cw"></a>

You can monitor the metrics that a training job emits in real time in the CloudWatch console\.

**To monitor training job metrics \(CloudWatch console\)**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose **/aws/sagemaker/TrainingJobs**\.

1. Choose **TrainingJobName**\.

1. On the **All metrics** tab, choose the names of the training metrics that you want to monitor\.

1. On the **Graphed metrics** tab, configure the graph options\. For more information about using CloudWatch graphs, see [Graph Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_metrics.html) in the *Amazon CloudWatch User Guide*\.

## Monitoring Training Job Metrics \(SageMaker Console\)<a name="view-train-metrics-sm"></a>

You can monitor the metrics that a training job emits in real time by using the SageMaker console\.

**To monitor training job metrics \(SageMaker console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Training jobs**, then choose the training job whose metrics you want to see\.

1. Choose **TrainingJobName**\.

1. In the **Monitor** section, you can review the graphs of instance utilization and algorithm metrics\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/console-metrics.png)

## Example: Viewing a Training and Validation Curve<a name="train-valid-curve"></a>

Typically, you split the data that you train your model on into training and validation datasets\. You use the training set to train the model parameters that are used to make predictions on the training dataset\. Then you test how well the model makes predictions by calculating predictions for the validation set\. To analyze the performance of a training job, you commonly plot a training curve against a validation curve\. 

Viewing a graph that shows the accuracy for both the training and validation sets over time can help you to improve the performance of your model\. For example, if training accuracy continues to increase over time, but, at some point, validation accuracy starts to decrease, you are likely overfitting your model\. To address this, you can make adjustments to your model, such as increasing [regularization](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#regularization)\.

For this example, you can use the **Image\-classification\-full\-training** example that is in the **Example notebooks** section of your SageMaker notebook instance\. If you don't have a SageMaker notebook instance, create one by following the instructions at [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\. If you prefer, you can follow along with the [End\-to\-End Multiclass Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-fulltraining.ipynb) in the example notebook on GitHub\. You also need an Amazon S3 bucket to store the training data and for the model output\. If you haven't created a bucket to use with SageMaker, create one by following the instructions at [Step 1: Create an Amazon S3 Bucket](gs-config-permissions.md)\.

**To view training and validation error curves**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Notebooks**, and then choose **Notebook instances**\.

1. Choose the notebook instance that you want to use, and then choose **Open**\.

1. On the dashboard for your notebook instance, choose **SageMaker Examples**\.

1. Expand the **Introduction to Amazon Algorithms** section, and then choose **Use** next to **Image\-classification\-full\-training\.ipynb**\.

1. Choose **Create copy**\. SageMaker creates an editable copy of the **Image\-classification\-full\-training\.ipynb** notebook in your notebook instance\.

1. In the first code cell of the notebook, replace *<<bucket\-name>>* with the name of your S3 bucket\.

1. Run all of the cells in the notebook up to the **Deploy** section\. You don't need to deploy an endpoint or get inference for this example\.

1. After the training job starts, open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, then choose **/aws/sagemaker/TrainingJobs**\.

1. Choose **TrainingJobName**\.

1. On the **All metrics** tab, choose the **train:accuracy** and **validation:accuracy** metrics for the training job that you created in the notebook\.

1. On the graph, choose an area that the metric's values to zoom in\. You should see something like the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/train-valid-acc.png)