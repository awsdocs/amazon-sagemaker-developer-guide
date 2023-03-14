# Create an Amazon SageMaker Experiment<a name="experiments-create"></a>

Create an Amazon SageMaker experiment to track your machine learning \(ML\) workflows with a few lines of code from your preferred development environment\. You can then browse your experiments, create visualizations for analysis, and find the best performing model\. You can also integrate SageMaker Experiments into your SageMaker training script using the SageMaker Python SDK\.

## Overview<a name="experiments-create-overview"></a>

The following components make up the building blocks of an experiment in Amazon SageMaker\.
+ `experiment`: An experiment is a collection of runs\. When you initialize a run in your training loop, you include the name of the experiment that the run belongs to\. Experiment names must be unique within your AWS account\. 
+ `Run`: A run consists of all the inputs, parameters, configurations, and results for one interaction of model training\. Initialize an experiment run for tracking a training job with `Run.init()`\.
**Note**  
We recommend that you initialize a `Run` object in a Jupyter Notebook, and create the SageMaker job for your experiment within the context of this `Run` object initialization\. To refer to this `Run` object in script mode, use the `load_run()` method\. For examples, see [Example notebooks for Amazon SageMaker Experiments](experiments-tutorials.md)\.
**Note**  
The SageMaker Python SDK automatically turns experiment names and run names to lowercase\.
+ `load_run`: To run your experiments in script mode, refer to an initialized `Run` object with `load_run()`\. If an experiment for a run exists, `load_run` returns the experiment context\. Generally, you use `load_run` with no arguments to track metrics, parameters, and artifacts within a SageMaker training or processing job script\. 

  ```
  # Load run from a local script passing experiment and run names
  with load_run(experiment_name=experiment_name, run_name=run_name) as run:
     run.log_parameter("param1", "value1")
  ```

  ```
  # Load run within a training or processing Job (automated context sharing)
  with load_run() as run:
      run.log_parameter("param1", "value1")
  ```
+ `log_parameter`: Log parameters for a run, such as batch size or epochs, over time in a training loop with `run.log_parameter()`\. `log_parameter` records a single name\-value pair in a run\. You can use `run.log_parameters()` to log multiple parameters\. If called multiple times within a run for a parameter of the same name, `log_parameter` overwrites any previous value\. The name must be a string and the value must be either a string, integer, or float\.

  ```
  # Log a single parameter
  run.log_parameter("param1", "value1")
  ```

  ```
  # Log multiple parameters
  run.log_parameters({
      "param2": "value2",
      "param3": "value3"
  })
  ```
+ `log_metric`: Log metrics for a run, such as accuracy or loss, over time in a training loop with `run.log_metric()`\. `log_metric` records a name\-value pair where the name is a string and the value is an integer or float\. To declare the frequency of logging over the course of the run, define a `step` value\. You can then visualize these metrics in the Studio Experiments UI\. For more information, see [View, search, and compare experiment runs](experiments-view-compare.md)\.

  ```
  # Log a metric over the course of a run
  run.log_metric(name="Final_loss", value=finalloss)
  ```

  ```
  # Log a metric over the course of a run at each epoch
  run.log_metric(name="test:loss", value=loss, step=epoch)
  ```
+ `log_artifact`: Log any input or output artifacts related to a run with `run.log_artifact()`\. Log artifacts such as S3 URIs, datasets, models, and more for your experiment to help you keep track of artifacts across multiple runs\. `is_output` is `True` by default\. To record the artifact as an input artifact instead of an output artifact, set `is_output` to `False`\.

  ```
  # Track a string value as an input or output artifact
  run.log_artifact(name="training_data", value="data.csv" is_output=False)
  ```
+ `log_file`: Log any input or output files related to a run, such as training or test data, and store them in Amazon S3 with `run.log_file()`\. `is_output` is `True` by default\. To record the file as an input artifact instead of an output artifact, set `is_output` to `False`\.

  ```
  # Upload a local file to S3 and track it as an input or output artifact
  run.log_file("training_data.csv", name="training_data", is_output=False)
  ```

For more information on initializing a `Run` object, see [Experiments](https://sagemaker.readthedocs.io/en/stable/experiments/sagemaker.experiments.html) in the *SageMaker Python SDK documentation*\. For information on visualizing logged experiment data and automatic logging, see [View, search, and compare experiment runs](experiments-view-compare.md)\.

## Create an experiment with the SageMaker Python SDK<a name="experiments-create-python-sdk"></a>

The following section demonstrates how to create an Amazon SageMaker Experiment using the SageMaker Python SDK\. This example uses the `Run` class to track a Keras model in a notebook environment\. The Keras `Callback` class provides a method `on_epoch_end` which emits metrics at the end of each epoch\. First, define a `Callback` class\.

```
class ExperimentCallback(keras.callbacks.Callback):
    """ """

    def __init__(self, run, model, x_test, y_test):
        """Save params in constructor"""
        self.run = run
        self.model = model
        self.x_test = x_test
        self.y_test = y_test

    def on_epoch_end(self, epoch, logs=None):
        """ """
        keys = list(logs.keys())
        for key in keys:
            run.log_metric(name=key, value=logs[key], step=epoch)
            print("Epoch: {}\n{} -> {}".format(epoch, key, logs[key]))
```

Next, train the Keras model in a notebook environment and track it as an experiment\. 

**Note**  
This example carries out jobs sequentially\. To run SageMaker jobs asynchronously, you may need to increase your resource limit\.

```
from sagemaker.experiments import Run

# The run name is an optional argument to `run.init()`
with Run(experiment_name = 'my-experiment') as run:
    
    # Define values for the parameters to log
    run.log_parameter("batch_size", batch_size)
    run.log_parameter("epochs", epochs)
    run.log_parameter("dropout", 0.5)
    
    # Define input artifacts
    run.log_file('datasets/input_train.npy', is_output = False)
    run.log_file('datasets/input_test.npy', is_output = False)
    run.log_file('datasets/input_train_labels.npy', is_output = False)
    run.log_file('datasets/input_test_labels.npy', is_output = False)

    # Train locally
    model.fit(
        x_train, 
        y_train, 
        batch_size=batch_size, 
        epochs=epochs, 
        validation_split=0.1, 
        callbacks = [ExperimentCallback(run, model, x_test, y_test)]
    )
    
    score = model.evaluate(x_test, y_test, verbose=0)
    print("Test loss:", score[0])
    print("Test accuracy:", score[1])
    
    # Define metrics to log
    run.log_metric(name = "Final Test Loss", value = score[0])
    run.log_metric(name = "Final Test Loss", value = score[1])
```

For more code samples and example notebooks, see [Example notebooks for Amazon SageMaker Experiments](experiments-tutorials.md)\.

## Create an experiment using SageMaker script mode<a name="experiments-create-script-mode"></a>

You can use SageMaker script mode to write your own code to train a model and track it as an experiment\. When creating an experiment with script mode, use `load_run()`\.

```
# Make sure that you have the latest version of the SageMaker Python SDK
import os
os.system("pip install -U sagemaker")

# Import additional requirements
import boto3
from sagemaker.session import Session
from sagemaker.experiments.run import load_run

# Define training script
if __name__ == "__main__":
    session = Session(boto3.session.Session(region_name=args.region))
    with load_run(sagemaker_session=session) as run:
        # Define values for the parameters to log
        run.log_parameters({
            "batch_size": batch_size,
            "epochs": epochs,
            "dropout": 0.5
        })
        # Define input artifacts
        run.log_file('datasets/input_train.npy', is_output = False)
        run.log_file('datasets/input_test.npy', is_output = False)
        run.log_file('datasets/input_train_labels.npy', is_output = False)
        run.log_file('datasets/input_test_labels.npy', is_output = False)
        
        # Train the model
        model.fit(
            x_train, 
            y_train, 
            batch_size=batch_size, 
            epochs=epochs, 
            validation_split=0.1, 
            callbacks = [ExperimentCallback(run, model, x_test, y_test)]
        )
        
        score = model.evaluate(x_test, y_test, verbose=0)
        print("Test loss:", score[0])
        print("Test accuracy:", score[1])
        
        # Define metrics to log
        run.log_metric(name = "Final Test Loss", value = score[0])
        run.log_metric(name = "Final Test Loss", value = score[1])
```

For more code samples and example notebooks on using Amazon SageMaker Experiments in SageMaker script mode, see [Track experiments for SageMaker training jobs using script mode](experiments-tutorials.md#experiments-tutorials-scripts)\.

For more information on script mode, see [Use script mode in a supported framework](https://docs.aws.amazon.com/sagemaker/latest/dg/algorithms-choose.html#supported-frameworks-benefits)\. You can also define custom metrics in script mode by specifying a name and regular expression for each metric that a tuning job monitors\. See [Use a custom algorithm for training](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-define-metrics.html#automatic-model-tuning-define-metrics-custom) for more information\.

## View your experiment in Studio<a name="experiments-create-view"></a>

To view the experiment in Studio, in the left sidebar, choose **Experiments**\.

Select the name of the experiment to view all associated runs\. It might take a moment for the list to refresh and display a new experiment or experiment run\. You can click **Refresh** to update the page\. Your experiment list should look similar to the following:

![\[A list of experiments in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-overview.png)

To view the runs that make up your experiment, select the experiment name\. For more information, see [View, search, and compare experiment runs](experiments-view-compare.md)\.

### View unassigned runs<a name="experiments-create-view-unassigned"></a>

All SageMaker jobs, including training jobs, processing jobs, and transform jobs, correspond to runs and create `Run` objects by default\. If you launch these jobs without explicitly associating them with an experiment, the resulting runs are unassigned and can be viewed in the **Unassigned runs** section of the Studio Experiments UI\.

![\[A list of unassigned runs in the SageMaker Experiments UI\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-unassigned-runs.png)

To clean up the resources you created, see [Clean Up Amazon SageMaker Experiment Resources](experiments-cleanup.md)\.