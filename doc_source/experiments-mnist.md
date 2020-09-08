# Track and Compare Tutorial<a name="experiments-mnist"></a>

This tutorial demonstrates how to visually track and compare trials in a model training experiment using Amazon SageMaker Studio\. The basis of the tutorial is the MNIST Handwritten Digits Classification Experiment \(MNIST\) example notebook\.

It is intended that this topic be viewed alongside Studio with the MNIST notebook open\. As you run through the cells, the sections in this document highlight the relevant code and show you how to observe the results in Studio\. Some of the code snippets have been edited for brevity\.

To clean up the resources created by the notebook, see [Clean Up Amazon SageMaker Experiment Resources](experiments-cleanup.md)\.

For a tutorial that showcases additional features of Studio, see [Amazon SageMaker Studio Tour](gs-studio-end-to-end.md)\.

**Prerequisites**
+ A local copy of the [MNIST](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/sagemaker_experiments/mnist-handwritten-digits-classification-experiment/mnist-handwritten-digits-classification-experiment.ipynb) example notebook and the companion [mnist\.py](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/sagemaker_experiments/mnist-handwritten-digits-classification-experiment/mnist.py) file\. Both files are available from the `aws_sagemaker_studio/sagemaker_experiments/mnist-handwritten-digits-classification-experiment` folder in the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository\. To download the files, choose each link, right\-click on the **Raw** button, and then choose **Save as**\.
+ An AWS SSO or IAM account to sign\-on to SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

**Topics**
+ [Open the Notebook in Studio](#experiments-mnist-notebook)
+ [Install the Experiments SDK and Import Modules](#experiments-mnist-setup)
+ [Transform and Track the Input Data](#experiments-mnist-s3bucket)
+ [Create and Track an Experiment](#experiments-mnist-experiment)
+ [Compare and Analyze Trials](#experiments-mnist-compare-trials)

## Open the Notebook in Studio<a name="experiments-mnist-notebook"></a>

**To open the notebook**

1. Sign\-on to Studio\.

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \)\.

1. At the top of the file browser pane, choose the **Up arrow** icon and then a **File Upload** dialog opens\. Browse to and choose your local versions of the *mnist\-handwritten\-digits\-classification\-experiment\.ipynb* and *mnist\.py* files, and then choose **Open**\.

1. The two files are listed in the file browser\. Double\-click the uploaded notebook file to open the notebook in a new tab\.

1. At the top right of the notebook, make sure the kernel is **Python 3 \(Data Science\)**\. If not, choose the current kernel name to open the **Select Kernel** dropdown\. Choose **Python 3 \(Data Science\)** and then choose **Select**\.

## Install the Experiments SDK and Import Modules<a name="experiments-mnist-setup"></a>

The SageMaker Experiments SDK is separate from the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), which comes preinstalled in SageMaker Studio\. Run the first few cells in the notebook to install the Experiments SDK and import the Experiments modules\. The relevant sections of the notebook cells are displayed below\. For more information on the Experiments SDK, see [sagemaker\-experiments](https://github.com/aws/sagemaker-experiments)\.

```
import sys
!{sys.executable} -m pip install sagemaker-experiments
```

```
import sagemaker
from sagemaker import get_execution_role
from sagemaker.session import Session
from sagemaker.analytics import ExperimentAnalytics

from smexperiments.experiment import Experiment
from smexperiments.trial import Trial
from smexperiments.trial_component import TrialComponent
from smexperiments.tracker import Tracker
```

## Transform and Track the Input Data<a name="experiments-mnist-s3bucket"></a>

The next few cells create an Amazon S3 bucket and a folder in the bucket named *mnist*\. In Studio, the file browser displays the *mnist* folder\. The input data is downloaded to the *mnist/MNIST/raw* folder, normalized, and then the transformed data is uploaded to the *mnist/MNIST/processed* folder\. You can drill down into the *mnist* folder to display, but not open, the data files\.

Your screen should look similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-s3-files.png)

The last cell in the **Dataset** section creates a [Tracker](https://github.com/aws/sagemaker-experiments/blob/master/src/smexperiments/tracker.py) for the transform job\. The tracker logs the normalization parameters and the URI of the Amazon S3 bucket where the transformed dataset is stored\. In a later section, we show how to find this information in Studio\. In the next section, the tracker is used to track the experiment and trial runs\.

```
with Tracker.create(display_name="Preprocessing", sagemaker_boto_client=sm) as tracker:
    tracker.log_parameters({
        "normalization_mean": 0.1307,
        "normalization_std": 0.3081,
    })
    tracker.log_input(name="mnist-dataset", media_type="s3/uri", value=inputs)
```

## Create and Track an Experiment<a name="experiments-mnist-experiment"></a>

The following procedure creates and tracks an experiment to determine the effect of the model's `num_hidden_channel` hyperparameter\. As part of the experiment, five trials are created inside a loop, one for each value of the `num_hidden_channel` hyperparameter\. Later in the notebook, you'll compare the results of these five trials\.

1. In the left sidebar of Studio, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \) to display the experiments browser\.

1. Run the following cell\.

   ```
   mnist_experiment = Experiment.create(
       experiment_name=f"mnist-hand-written-digits-classification-{int(time.time())}",
       description="Classification of mnist hand-written digits", 
       sagemaker_boto_client=sm)
   print(mnist_experiment)
   ```

   Output:

   ```
   Experiment(sagemaker_boto_client=<botocore.client.SageMaker object at 0x7f7152b326d8>,
              experiment_name='mnist-hand-written-digits-classification-1575947870',
              description='Classification of mnist hand-written digits',
              experiment_arn='arn:aws:sagemaker:us-east-2:acct-id:experiment/mnist-hand-written-digits-classification-1575947870')
   ```

   After the code runs, the experiments list contains an entry for the experiment\. It might take a moment to display and you might have to refresh the experiments list\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-experiment.png)

1. Run the following cell\.

   ```
   preprocessing_trial_component = tracker.trial_component
   ```

   After the code runs, the experiments list contains an entry labeled **Unassigned trial components**\. The trial component entry is the data preprocessing step previously created\. Double\-click the trial component for verification\. The trial component isn't associated with an experiment at this time\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-unassigned-tc-preprocess.png)

1. Choose the `Home` icon in the navigation breadcrumb at the top on the experiments browser\. From there, double\-click on your experiment to display a list of the trials in the experiment\.

1. The following code create trials for the experiment\. Each trial trains a model using a different number for the `num_hidden_channel` hyperparameter\. The preprocessing trial component is added to each trial for complete tracking \(for example, for auditing purposes\)\. The code also specifies definitions for the following metrics:
   + Train loss
   + Test loss
   + Test accuracy

   The definitions tell SageMaker to capture those metrics from the algorithm's log output\. The metrics are used later to evaluate and compare the models\.

   ```
   preprocessing_trial_component = tracker.trial_component
   
   for i, num_hidden_channel in enumerate([2, 5, 10, 20, 32]):
       trial_name = f"cnn-training-job-{num_hidden_channel}-hidden-channels-{int(time.time())}"
       cnn_trial = Trial.create(
           trial_name=trial_name,
           experiment_name=mnist_experiment.experiment_name,
           sagemaker_boto_client=sm,
       )
       hidden_channel_trial_name_map[num_hidden_channel] = trial_name
       
       cnn_trial.add_trial_component(preprocessing_trial_component)
       
       estimator = PyTorch(
           hyperparameters={
               'hidden_channels': num_hidden_channel,
               ...
           },
           metric_definitions=[
               {'Name':'train:loss', 'Regex':'Train Loss: (.*?);'},
               {'Name':'test:loss', 'Regex':'Test Average loss: (.*?),'},
               {'Name':'test:accuracy', 'Regex':'Test Accuracy: (.*?)%;'}
           ],
           enable_sagemaker_metrics=True,
       )
       
       cnn_training_job_name = "cnn-training-job-{}".format(int(time.time()))
       
       estimator.fit(
           inputs={'training': inputs}, 
           job_name=cnn_training_job_name,
           experiment_config={
               "TrialName": cnn_trial.trial_name,
               "TrialComponentDisplayName": "Training",
           },
       )
   ```

   The trial list automatically updates as each training job runs\. It takes a few minutes for each trial to be displayed\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-trials.png)

## Compare and Analyze Trials<a name="experiments-mnist-compare-trials"></a>

This section deviates from the notebook and shows you how to compare and analyze the trained models using the SageMaker Studio UI\.

**To view the details of a trial**

1. Double\-click one of the trials to display a list of the trial components associated with the trial\. There's a preprocessing job and training job for each trial\. Double\-click one of the components to open a new tab that displays information about each component\.

1. Under **Trial stages**, choose **Preprocessing**\. On the **Describe Trial Component** menu, choose **Parameters** to display the normalization parameters that were previously logged\. Next, choose **Artifacts** to display the URI of the Amazon S3 bucket where the transformed dataset was stored\.

1. Under **Trial stages**, choose **Training**\. On the **Describe Trial Component** menu, choose the following items to display information about the training job trial component\.
   + **Metrics** – `test:loss`, `test:accuracy`, and `train:loss`
   + **Parameters** – hyperparameter values and instance information
   + **Artifacts** – Amazon S3 storage for the input dataset and the output model
   + **AWS Settings** – job name, ARN, status, creation time, training time, billable time, instance information, and others

**To view a list of trials ordered by `test:accuracy`**

1. Choose the experiment name on the navigation breadcrumb above **TRIAL COMPONENTS** to display the trial list\.

1. Choose all five trials\. Hold down the CTRL/CMD key and select each trial\. Right\-click the selection and then choose **Open in trial component list**\. A new tab opens that displays each trial and trial component\.

1. If the **TABLE PROPERTIES** pane isn't open, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) in the upper right corner to open it\. Deselect everything except **Trial**, **Metrics**, and **Training job**\. Choose the **Settings** icon to close the pane\.

1.  Choose the **test:accuracy** column header to sort the list by decreasing maximum test accuracy\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-trials-accuracy.png)

**To view a chart of `test:loss` versus `num_hidden_channel`**

1. In the **TRIAL COMPONENTS** pane, choose all five trials and then choose **Add chart**\. Select inside the chart area to open the preferences pane for **CHART PROPERTIES**\.

1. In **CHART PROPERTIES**, choose the following:
   + **Data type** \- **Summary statistics**
   + **Chart type** \- **Line**
   + **X\-axis** \- **hidden\-channels**
   + **Y\-axis** \- **test:lost\_last**
   + **Color** \- **None**

   Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-test-lost-chart-props.png)