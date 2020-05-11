# Amazon SageMaker Studio tour<a name="gs-studio-end-to-end"></a>

This example shows how to use the main features of Amazon SageMaker Studio\. The example is based on the [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/aws\_sagemaker\_studio/getting\_started/xgboost\_customer\_churn\_studio\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/getting_started/xgboost_customer_churn_studio.ipynb) sample Jupyter notebook\. You must run this sample notebook in Studio\.

For a series of videos that shows how to use the main features of Amazon SageMaker Studio, see [NEW\! Amazon SageMaker Studio](https://www.youtube.com/playlist?list=PLJgojBtbsuc0MjdtpJPo4g4PL8mMsd2nK) on YouTube\.

**Prerequisites**

To run the code for this tour, you need:
+ An AWS SSO or IAM account to sign on to Studio\. For information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.
+ A copy of the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository in your Studio environment

**To start the tour**

  1. Sign in to Amazon SageMaker Studio\. For instructions, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

  1. On the **Launcher** page, choose **Terminal** to launch a command terminal\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_terminal.png)

  1. At the command prompt, run the following command to clone the amazon\-sagemaker\-examples repository\.

     `git clone https://github.com/awslabs/amazon-sagemaker-examples.git`

**Topics**
+ [Use Amazon SageMaker Studio Notebooks](#studio-tour-notebooks)
+ [Keep Track of Machine Learning Experiments](#studio-tour-experiments)
+ [Create Charts to Visualize Data](#studio-create-chart)
+ [Debug Training Jobs](#studio-tour-debug)
+ [Monitor Deployed Models](#studio-tour-monitor)
+ [Create an Amazon SageMaker Autopilot Experiment](#studio-tour-autopilot)

## Use Amazon SageMaker Studio Notebooks<a name="studio-tour-notebooks"></a>

Amazon SageMaker Studio notebooks are collaborative Jupyter notebooks that are built into Amazon SageMaker Studio\. You can launch Studio notebooks without setting up compute instances and file storage, so you can get started fast\. You pay only for the resources used when you run the notebooks\.

You can share notebooks with others in your organization, so that they can easily reproduce your results and collaborate while building models and exploring your data\. By default, metadata for the image that you used with your notebook is included in the notebook's image settings\. It provides your colleagues access to the notebook through a secure URL\. When they copy and edit the notebook, the same image that you used will be selected by default\. If you make any changes to the image \(for example, conda or pip installs\), these are not shared\.

For more information about Amazon SageMaker Studio notebooks, see [Use Amazon SageMaker Studio Notebooks](notebooks.md)\.

**To open the notebook**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

1. Choose the file browser icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \)\.

1. Navigate to `amazon-sagemaker-examples/aws_sagemaker_studio/getting_started`\.

1. Choose **xgboost\_customer\_churn\_studio\.ipynb**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_notebook.png)

## Keep Track of Machine Learning Experiments<a name="studio-tour-experiments"></a>

Organize, track, compare, and evaluate your machine learning experiments with Amazon SageMaker Experiments\. When you create a training job in Amazon SageMaker, you can include code that creates a *trial* for the job and associates the trial with an experiment\. You can then use Amazon SageMaker Studio to organize and track your experiments\. For more information about how to use Amazon SageMaker Experiments, see [Track and compare trials in Amazon SageMaker Studio](experiments-mnist.md)\.

**To create an experiment with training trials**

1. Run the following cell to create an experiment by calling the `create` method of the `Experiment` class\.

   ```
   sess = sagemaker.session.Session()
   
   create_date = strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   customer_churn_experiment = Experiment.create(experiment_name="customer-churn-prediction-xgboost-{}".format(create_date), 
                                                 description="Using xgboost to predict customer churn", 
                                                 sagemaker_boto_client=boto3.client('sagemaker'))
   ```

1. Create trials and associate them with the experiment that you created in the previous step, and then run the trials by calling the `fit` method of the `Estimator` class\. Here, we'll look at the cell in the notebook named `Trying other hyperparameter values so that we generate several trials.` 

   ```
   min_child_weights = [1, 2, 4, 8, 10]
   
   for weight in min_child_weights:
       hyperparams["min_child_weight"] = weight
       trial = Trial.create(trial_name="algorithm-mode-trial-{}-weight-{}".format(strftime("%Y-%m-%d-%H-%M-%S", gmtime()), weight), 
                            experiment_name=customer_churn_experiment.experiment_name,
                            sagemaker_boto_client=boto3.client('sagemaker'))
   
       t_xgb = sagemaker.estimator.Estimator(image_name=docker_image_name,
                                             role=role,
                                             hyperparameters=hyperparams,
                                             train_instance_count=1, 
                                             train_instance_type='ml.m4.xlarge',
                                             output_path='s3://{}/{}/output'.format(bucket, prefix),
                                             base_job_name="demo-xgboost-customer-churn",
                                             sagemaker_session=sess)
   
       t_xgb.fit({'train': s3_input_train,
                  'validation': s3_input_validation},
                   wait=False,
                   experiment_config={
                       "ExperimentName": customer_churn_experiment.experiment_name, 
                       "TrialName": trial.trial_name,
                       "TrialComponentDisplayName": "Training",
                   }
                  )
   ```

After the training jobs finish, you can see the new experiment and trials in the Studio experiments list by choosing the SageMaker Experiment List icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_experiments_icon.png)

To see the trials associated with your experiment, open the context \(right click\) menu on the experiment name and choose **Open in trial component list**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_experiment_list.png)

On this page, you can see the details about the trials, compare trials to find the best performing model, and create charts to visualize training results\.

## Create Charts to Visualize Data<a name="studio-create-chart"></a>

To visualize data after training jobs run, you can create charts in Amazon SageMaker Studio\. In this example notebook, the training jobs run for a very short time, so they don't create much data\. Because of this, we'll create a scatter plot of the `validation:error_last` metric \(final validation error\) for each of the `min_child_weight` hyperparameter values that we specified in the training jobs\.

**To create the scatter plot**

1. In Studio, choose the SageMaker Experiment List icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \)\.

1. Open the context \(right\-click\) menu for the experiment, and then choose **Open in trial component list**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_experiment_list.png)

1. In the **Trial Components** list, multi\-select the five trials that we varied the `min_child_weight` for, then choose **Add chart**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_add_chart.png)

1. Choose **Settings**, then configure the chart properties as follows:
   + For **Data type**, choose **Summary statistics**\.
   + For **Chart type**, choose **Scatter plot**\.
   + For **X\-axis**, choose **min\_child\_weight**\.
   + For **Y\-axis**, choose **validation:error\_last**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_chart_settings.png)

   Studio displays the scatter plot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_scatter_plot.png)

## Debug Training Jobs<a name="studio-tour-debug"></a>

Amazon SageMaker Debugger helps you analyze your training jobs and find problems\. It monitors, records, and analyzes tensor data from training jobs and checks the training tensors against a set of rules that you specify\. You can choose from a list of built\-in rules, or create your own custom rules\. For more information, see [Amazon SageMaker Debugger](train-debugger.md)\.

**To use Amazon SageMaker Debugger to debug a training job**

1. Specify one or more rules that you want to use to analyze your training job\. In this notebook, we use the following rules\.

   ```
   debug_rules = [Rule.sagemaker(rule_configs.loss_not_decreasing()),
                  Rule.sagemaker(rule_configs.overtraining()),
                  Rule.sagemaker(rule_configs.overfit())
                 ]
   ```

1. Create a trial to track the new training job, and run the training job with the specified Debugger rules\. 

   ```
   entry_point_script = "xgboost_customer_churn.py"
   
   trial = Trial.create(trial_name="framework-mode-trial-{}".format(strftime("%Y-%m-%d-%H-%M-%S", gmtime())), 
                        experiment_name=customer_churn_experiment.experiment_name,
                        sagemaker_boto_client=boto3.client('sagemaker'))
   
   framework_xgb = sagemaker.xgboost.XGBoost(image_name=docker_image_name,
                                             entry_point=entry_point_script,
                                             role=role,
                                             framework_version="0.90-2",
                                             py_version="py3",
                                             hyperparameters=hyperparams,
                                             train_instance_count=1, 
                                             train_instance_type='ml.m4.xlarge',
                                             output_path='s3://{}/{}/output'.format(bucket, prefix),
                                             base_job_name="demo-xgboost-customer-churn",
                                             sagemaker_session=sess,
                                             rules=debug_rules
                                             )
   
   framework_xgb.fit({'train': s3_input_train,
                      'validation': s3_input_validation}, 
                     experiment_config={
                         "ExperimentName": customer_churn_experiment.experiment_name, 
                         "TrialName": trial.trial_name,
                         "TrialComponentDisplayName": "Training",
                     })
   ```

You can see the results for each Debugger rule that you specified by navigating in Studio to the trial that you just created in the **Trial Components** list, and then choosing **Debugger** on the **Describe Trial Component** page for the training job\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_debug.png)

Notice that the training job passed all three of the rules that were configured for the job\. If Debugger had found issues, you could choose the rule in the list to see more information in the **Debugger Details** page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_debug.png)

## Monitor Deployed Models<a name="studio-tour-monitor"></a>

Monitor the quality of your deployed models with Amazon SageMaker Model Monitor\. Model Monitor runs monitoring jobs on the endpoints where models are deployed\. You can use its built\-in monitoring capabilities, which don't require coding, or you can write code for custom analysis\. In this exercise, we'll look only at the results of a scheduled monitor job for the endpoint that you created in the example notebook\. For information about what to do to monitor models in production, see [Amazon SageMaker Model Monitor](model-monitor.md)\.

After you set up and schedule model monitoring, you can see the results in Amazon SageMaker Studio\.

**To monitor a deployed model**

1. In Studio, choose the **SageMaker Endpoint List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Endpoint_squid.png) \)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_monitor.png)

1. Choose the endpoint that you scheduled monitoring for\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_monitor_issues.png)

In the **Monitoring job history** list, you can see any issues that the monitoring jobs found\. To see details about an issue, choose the issue\.

## Create an Amazon SageMaker Autopilot Experiment<a name="studio-tour-autopilot"></a>

If the dataset that you want to train on contains tabular data that is formatted as comma\-separated values \(CSV\), you can use Amazon SageMaker Autopilot in Amazon SageMaker Studio to create models\. Autopilot simplifies the machine learning process by helping you explore your data by trying different algorithms\. It also automatically trains and tunes models on your behalf, to help you find the best algorithm\. For more information about Amazon SageMaker Autopilot, see [Use Amazon SageMaker Autopilot to Automate Model Development](autopilot-automate-model-development.md)\.

**To create an Autopilot experiment**

1. In Studio, on the **Experiments** pane, choose **Create Experiment**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_create_experiment.png)

1. On the **Create experiment** page, provide the required information for the Autopilot experiment:
   + A name for the experiment\.
   + The location in Amazon S3 where the input dataset is stored\.
**Note**  
The input data must be in CSV format and contain at least 1000 rows\.
   + The target attribute in the dataset that you want the model to predict\.
   + The location in Amazon S3 where you want to store the model output data\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_autopilot_values.png)