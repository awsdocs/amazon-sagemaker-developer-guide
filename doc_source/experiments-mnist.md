# Track and Evaluate a Model Training Experiment<a name="experiments-mnist"></a>

This tutorial demonstrates how to visually track, compare, and evaluate a model training experiment using Amazon SageMaker Studio\. The basis of the tutorial is the MNIST Handwritten Digits Classification Experiment \(MNIST\) example notebook\.

It is intended that this topic be viewed alongside Studio with the MNIST notebook open\. As you run through the cells, the sections highlight the relevant code and show you how to observe the results in Studio\. Some of the code snippets have been edited for brevity\.

**Prerequisites**
+ A local copy of the [MNIST](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/sagemaker_experiments/mnist-handwritten-digits-classification-experiment.ipynb) example notebook and the companion [mnist\.py](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/sagemaker_experiments/mnist.py) file\. Both are available from the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository\. To download the files, select each link, right\-click on the **Raw** button, and then choose **Save as**\.
+ An AWS SSO or IAM account to sign\-on to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

**Topics**
+ [Open the Notebook in Studio](#experiments-mnist-notebook)
+ [Set Up Amazon SageMaker Experiments](#experiments-mnist-setup)
+ [Create and Track an Experiment](#experiments-mnist-experiment)
+ [Compare Trials and Analyze](#experiments-mnist-compare-trials)
+ [Deploy the Top Model](#experiments-mnist-deploy)
+ [Clean Up Resources](#experiments-mnist-cleanup)

## Open the Notebook in Studio<a name="experiments-mnist-notebook"></a>

**To open the notebook**

1. Sign\-on to Studio\.

1. In the left sidebar, select the **File Browser** icon\.

1. At the top of the File Browser, select the **Up arrow** icon and then a **File Upload** dialog opens\. Browse to and select your local versions of the *mnist\-handwritten\-digits\-classification\-experiment\.ipynb* and *mnist\.py* files, and then choose **Open**\.

1. Double\-click the uploaded notebook file to open the notebook in a new tab\.

## Set Up Amazon SageMaker Experiments<a name="experiments-mnist-setup"></a>

The Amazon SageMaker Experiments SDK is separate from the Amazon SageMaker Python SDK\. In the following steps you install the Experiments SDK and import the relevant modules\. For more information on the Experiments SDK, see [sagemaker\-experiments](https://github.com/aws/sagemaker-experiments)\.

**To install the SDK and import modules**

1. Install the Experiments SDK\.

   ```
   !{sys.executable} -m pip install sagemaker-experiments
   ```

1. Import the Amazon SageMaker and Experiments modules\.

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

1. Run the remaining cells until you come to the last cell in the **Dataset** section\.

## Create and Track an Experiment<a name="experiments-mnist-experiment"></a>

The following code creates and tracks an experiment\. First, a [Tracker](https://github.com/aws/sagemaker-experiments/blob/master/src/smexperiments/tracker.py) is created and used to track the dataset transform job\. Next, an experiment is created and then 5 trials are created inside a loop, one for each value of the `num_hidden_channel` hyperparameter you're testing\.

1. In the left sidebar, select the **SageMaker Experiment List** icon to display the experiments browser\.

1. In the notebook, create a `Tracker` as a preprocessing step to track the transform job for the dataset\. The tracker logs the normalization parameters and the URI to the Amazon S3 bucket where the transformed dataset is uploaded\.

   ```
   with Tracker.create(display_name="Preprocessing", sagemaker_boto_client=sm) as tracker:
       tracker.log_parameters({
           "normalization_mean": 0.1307,
           "normalization_std": 0.3081,
       })
       tracker.log_input(name="mnist-dataset", media_type="s3/uri", value=inputs)
   ```

   ```
   preprocessing_trial_component = tracker.trial_component
   ```

   After the previous code runs, the experiments list contains an entry named **Unassigned trial components**\. This is the preprocessing step just created\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-unassigned-tc.png)

1. Create and start tracking an experiment\.

   ```
   mnist_experiment = Experiment.create(
       experiment_name=f"mnist-hand-written-digits-classification-{int(time.time())}",
       description="Classification of mnist hand-written digits", 
       sagemaker_boto_client=sm)
   print(mnist_experiment)
   ```

   ```
   Experiment(sagemaker_boto_client=<botocore.client.SageMaker object at 0x7f7152b326d8>,
              experiment_name='mnist-hand-written-digits-classification-1575947870',
              description='Classification of mnist hand-written digits',
              experiment_arn='arn:aws:sagemaker:us-east-2:acct-id:experiment/mnist-hand-written-digits-classification-1575947870')
   ```

   After the previous code runs, the experiments list contains an entry for the experiment\. It might take a moment to display\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-experiment.png)

1. Double\-click on your experiment to display a list of the trials in the experiment\. Initially the list is empty\.

1. Create trials for the experiment\. Each trial trains a model using a different number for the `hidden_channels` hyperparameter\. The preprocessing trial component is added to each trial for complete tracking \(for example, for auditing purposes\)\. The code also specifies definitions for the following metrics:
   + Train loss
   + Test loss
   + Test accuracy

   The definitions tell Amazon SageMaker to capture those metrics from the algorithm's log output\. The metrics are used later to evaluate and compare the models\.

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

## Compare Trials and Analyze<a name="experiments-mnist-compare-trials"></a>

This section deviates from the notebook and show you how to compare and analyze the trained models using the Amazon SageMaker Studio UI\.

**To view a list of training jobs ordered by `test:accuracy`**

1. Select all 5 trials, right\-click the selection, and then choose **Open in trial component list**\. A new tab opens that displays a list of the components for all trials\. There's a preprocessing job and training job for each trial\.

1. If the **TABLE PROPERTIES** pane isn't open, select the gear icon in the upper right corner to open it\. Unselect the **Created on** and **Last modified** checkboxes\.

1. Right\-click the **test:accuracy** column header, choose **Data aggregation**, and then choose **Maximum**\. Choose the **test:accuracy** column header to sort the list by decreasing maximum test accuracy\. You can see that the models trained with the `hidden_channels` hyperparameter set to 2 and 20 give the highest test accuracy\. Due to the randomness of model training and the closeness of the accuracies, your results might differ\. Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-mnist-trials-accuracy.png)

## Deploy the Top Model<a name="experiments-mnist-deploy"></a>

Now that you've determined the most accurate training job, deploy the associated model to an Amazon SageMaker endpoint\.

**To deploy the model**

1. In the left sidebar, select the **SageMaker Endpoint List** icon to display the endpoints browser\.

1. Right\-click the top training job in the trial components list and choose **Deploy model**\. A new tab opens that displays the **Deploy model** page\.

1. Under **REQUIRED SETTINGS**, enter a name for the endpoint\. Keep the default values of `ml.m5.xlarge` for **Instance type** and `1` for **Instance count**\.

1. Choose **Deploy model**\.

## Clean Up Resources<a name="experiments-mnist-cleanup"></a>

To avoid incurring unnecessary charges, delete the resources you created after you're done with the tutorial\. You can't delete the Experiment resources through the Amazon SageMaker Management Console or the Studio UI\. The following code shows how to clean up these resources\.

To delete the experiment, first you must delete all trials in the experiment\. To delete a trial, first you must remove all trial components from the trial\. To delete a trial component, first you must remove the component from all trials\.

**Note**  
Trial components can exist independent of trials and experiments\. You might want keep them if you plan on further exploration\. If so, comment out `tc.delete()`

```
def cleanup(experiment):
    for trial_summary in experiment.list_trials():
        trial = Trial.load(sagemaker_boto_client=sm, trial_name=trial_summary.trial_name)
        for trial_component_summary in trial.list_trial_components():
            tc = TrialComponent.load(
                sagemaker_boto_client=sm,
                trial_component_name=trial_component_summary.trial_component_name)
            trial.remove_trial_component(tc)
            try:
                # comment out to keep trial components
                tc.delete()
            except:
                # tc is associated with another trial
                continue
            # to prevent throttling
            time.sleep(.5)
        trial.delete()
    experiment.delete()

cleanup(mnist_experiment)
```

For information on deleting your Amazon S3 buckets, see [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.

**To delete the notebook**

1. Select the file browser\.

1. Right\-click the notebook file and choose **Shut Down Kernel**\.

1. Right\-click the notebook file and choose **Delete**\.