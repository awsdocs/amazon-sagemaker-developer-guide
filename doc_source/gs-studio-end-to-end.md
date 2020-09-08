# Amazon SageMaker Studio Tour<a name="gs-studio-end-to-end"></a>

This walkthrough takes you on a tour of the main features of Amazon SageMaker Studio using the [xgboost\_customer\_churn\_studio\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/getting_started/xgboost_customer_churn_studio.ipynb) sample notebook from the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository\. It is intended that you proceed through the walkthrough and run the notebook in Studio at the same time\.

The code in the notebook trains multiple models and sets up the SageMaker Debugger and SageMaker Model Monitor\. The walkthrough shows you how to view the trials, compare the resulting models, show the debugger results, and deploy the best model using the SageMaker Studio UI\. You don't need to understand the code to follow this walkthrough\.

For a series of videos that shows how to use the main features of SageMaker Studio, see [NEW\! Amazon SageMaker Studio](https://www.youtube.com/playlist?list=PLJgojBtbsuc0MjdtpJPo4g4PL8mMsd2nK) on YouTube\.

**Prerequisites**

To run the notebook for this tour, you need:
+ An AWS SSO or IAM account to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.
+ Basic familiarity with the Studio user interface and Jupyter notebooks\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.
+ A copy of the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository in your Studio environment\.

**To clone the repository**

1. Sign in to SageMaker Studio\. For AWS SSO users, sign in using the URL from your invitation email\. For IAM users, follow these steps\.

   1. Sign in to the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

   1. Choose **Amazon SageMaker Studio** in the left navigation pane\.

   1. Choose **Open Studio** in the row next to your user name\.

1. On the top menu, choose **File** then **New** then **Terminal**\.

1. At the command prompt, run the following command\.

   `git clone https://github.com/awslabs/amazon-sagemaker-examples.git`

**Topics**
+ [Open the Amazon SageMaker Studio Notebook](#studio-tour-notebooks)
+ [Keep Track of the Machine Learning Experiment](#studio-tour-experiments)
+ [Create a Chart to Visualize Data](#studio-create-chart)
+ [Debug the Training Job](#studio-tour-debug)
+ [Deploy the Best Model](#studio-tour-deploy)
+ [Monitor the Deployed Model](#studio-tour-monitor)
+ [Clean Up Resources](#studio-tour-cleanup)

## Open the Amazon SageMaker Studio Notebook<a name="studio-tour-notebooks"></a>

Amazon SageMaker Studio notebooks are collaborative Jupyter notebooks that are built into SageMaker Studio\. You can launch Studio notebooks without setting up compute instances and file storage, so you can get started fast\.

You can share notebooks with others in your organization, so that they can easily reproduce your results and collaborate while building models and exploring your data\.

For more information about SageMaker Studio notebooks, see [Use Amazon SageMaker Studio Notebooks](notebooks.md)\.

**To open the `xgboost_customer_churn_studio` notebook**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

1. Choose the file browser icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \)\.

1. Navigate to `amazon-sagemaker-examples/aws_sagemaker_studio/getting_started`\.

1. Double\-click **xgboost\_customer\_churn\_studio\.ipynb** to open the notebook\.

1. In the **Select Kernel** dialog, choose **Python 3 \(Data Science\)**, then choose **Select**\.

Your screen should resemble the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_notebook.png)

Next, you use the notebook to create an experiment\.

## Keep Track of the Machine Learning Experiment<a name="studio-tour-experiments"></a>

Amazon SageMaker Experiments lets you organize, track, compare, and evaluate your machine learning experiments\. An *experiment* is composed of multiple *trials* with the same objective\. Each trial is composed of multiple *trial components* such as a preprocessing job and a training job\.

First you create an experiment, then you create a trial which assigns it to the experiment\. Next, you create a training job as a trial component and associate the component with the trial\. For more information, see [Manage Machine Learning with Amazon SageMaker Experiments](experiments-mnist.md)\.

**To create an experiment and a trial with a training job**

1. Scroll down the notebook and choose the section titled **Amazon SageMaker Experiments**\.

1. In the Studio main menu, choose **Run** and then **Run All Above Selected Cell**\.

1. Hold down the Shift key and press Enter to run the next code cell, which creates an experiment by calling the `create` method of the [Experiment](https://sagemaker-experiments.readthedocs.io/en/latest/experiment.html) class\.

   ```
   sess = sagemaker.session.Session()
   
   create_date = strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   customer_churn_experiment = Experiment.create(experiment_name="customer-churn-prediction-xgboost-{}".format(create_date),
                                                 description="Using xgboost to predict customer churn",
                                                 sagemaker_boto_client=boto3.client('sagemaker'))
   ```

1. In the left sidebar, choose the SageMaker Experiment List icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \) to see the experiment \(named `customer-churn-prediction-xgboost...`\) in the experiments list\. You might need to refresh the list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-tour-experiment.png)

1. Run the next two code cells\. The first cell defines the hyperparameters to use in the training job\. The second cell creates a trial that is assigned to the experiment that was created in the previous step\. Next, the cell creates a training job as a trial component, then runs the trial by calling the `fit` method of the [Estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html) class\. It can take several minutes for the training job to complete\.
**Note**  
The output of the training job includes a long list of messages like `[0]#011train-error:0.077154#011validation-error:0.099099`\. These aren't errors due to the training job but are the results from the model training process\.

   ```
   trial = Trial.create(trial_name="algorithm-mode-trial-{}".format(strftime("%Y-%m-%d-%H-%M-%S", gmtime())),
                        experiment_name=customer_churn_experiment.experiment_name,
                        sagemaker_boto_client=boto3.client('sagemaker'))
   
   xgb = sagemaker.estimator.Estimator(image_name=docker_image_name,
                                       role=role,
                                       hyperparameters=hyperparams,
                                       train_instance_count=1, 
                                       train_instance_type='ml.m4.xlarge',
                                       output_path='s3://{}/{}/output'.format(bucket, prefix),
                                       base_job_name="demo-xgboost-customer-churn",
                                       sagemaker_session=sess)
   
   xgb.fit({'train': s3_input_train,
            'validation': s3_input_validation}, 
            experiment_config={
               "ExperimentName": customer_churn_experiment.experiment_name, 
               "TrialName": trial.trial_name,
               "TrialComponentDisplayName": "Training",
           }
          )
   ```

1. In the experiments list, double\-click the experiment name to see the trial \(named `algorithm-mode-trial...`\)\.

1. Double\-click the trial name to see the associated trial component \(named `Training`\)\.

1. Double\-click the **Training** trial component to open the **Describe Trial Component** tab\. You can follow the progress of the training job here\.

After the trial finishes, you can see details about the training job, such as metrics and hyperparameters, charts that visualize the training results\. To see the billable time and instance type, choose the **AWS Settings** heading\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-tour-trial-component.png)

Next, the notebook creates and compares multiple trials that use different values for the `min_child_weight` hyperparameter\.

**To create and compare multiple trials**

1. Scroll to the section of the notebook titled **Trying other hyperparameter values**\.

1. Run the following cell that creates and runs five trials, each with a different value of the `min_child_weight` hyperparameter\.
**Note**  
In the previous step of creating a single trial, the output of the training job is displayed\. Here, the output is suppressed as it would display about three thousand lines\.

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

1. To follow the progress and view the results in Studio, choose the **Home** icon above **TRIAL COMPONENTS**\.

1. Right\-click the experiment name and choose **Open in trial component list**\. You can see details about the trials, compare trials to find the best performing model, and create charts to visualize training results\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-tour-trials.png)

After all the trials finish, sort the trials by choosing the **validation:error** heading\. In a later section, you will deploy the trial with the lowest `validation:error`\.

## Create a Chart to Visualize Data<a name="studio-create-chart"></a>

To visualize data after the training jobs run, you can create charts in Amazon SageMaker Studio\. In this notebook, the training jobs run for a very short time, so they don't create much data\. Because of this, you create a scatter plot of the `validation:error_last` metric \(final validation error\) for each of the `min_child_weight` hyperparameter values that were specified in the training jobs\.

**To create the scatter plot**

1. In the **TRIAL COMPONENTS** list, multi\-select the five trials from the previous step, then choose **Add chart**\.

1. If the **CHART PROPERTIES** pane isn't open, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) in the upper right corner to open it\. Choose the **Settings** icon again when you want to close the pane\.

1. Configure the chart properties as follows:
   + For **Data type**, choose **Summary statistics**\.
   + For **Chart type**, choose **Scatter plot**\.
   + For **X\-axis**, choose **min\_child\_weight**\.
   + For **Y\-axis**, choose **validation:error\_last**\.
   + For **Color**, choose **trialComponentName**\.

Studio displays the scatter plot\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_scatter_plot.png)

Next, the notebook sets up the SageMaker Debugger\.

## Debug the Training Job<a name="studio-tour-debug"></a>

Amazon SageMaker Debugger helps you analyze your training jobs and find problems\. It monitors, records, and analyzes tensor data from training jobs and checks the training tensors against a set of rules that you specify\. You can choose from a list of built\-in rules, or create your own custom rules\. For more information, see [Amazon SageMaker Debugger](train-debugger.md)\.

**To debug a training job**

1. To specify the rules to use to analyze your training job, run the following cell in the section titled **Amazon SageMaker Debugger**\.

   ```
   debug_rules = [Rule.sagemaker(rule_configs.loss_not_decreasing()),
                  Rule.sagemaker(rule_configs.overtraining()),
                  Rule.sagemaker(rule_configs.overfit())
                 ]
   ```

1. Run the remaining cells in the section to create a new trial using the debug rules\. Note the `rules=debug_rules` argument that is added to the `fit` call\.

   ```
   entry_point_script = "xgboost_customer_churn.py"
   
   trial = Trial.create(...)
   
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
   
   framework_xgb.fit(...)
   ```

1. In the experiments list, double\-click the experiment name to see the trials list\.

1. In the trials list, right\-click the debug trial \(named `framework-mode-trial...`\) and choose **Open in trial component list**\.

1. In the trial components list, right\-click the trial and choose **Open in trial details**\.

1. To see the results for each debug rule that you specified, choose the **Debugger** heading\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-tour-debug.png)

Notice that the training job passed all three of the rules that were configured for the job\. If Debugger had found issues, you could choose the rule in the list to see more information in the **Debugger Details** tab\.

Next, you deploy the model\.

## Deploy the Best Model<a name="studio-tour-deploy"></a>

You can create an endpoint and deploy a model using the SDK or the Amazon SageMaker Studio UI\. The notebook shows you how to deploy using the SDK\. For this tour, we show you how to deploy using the Studio UI\. After you deploy the model, you can set up the SageMaker Model Monitor to monitor the endpoint\.

**To deploy a model using the Studio UI**

1. In the experiments list, right\-click the experiment and choose **Open in trial component list**\.

1. To sort the trials, choose the **validation:error** heading\.

1. Right\-click the trial with the lowest `validation:error` and choose **Deploy model**\.

1. Under **Deploy model**, specify a name for the endpoint that will host the model\.

1. Choose **Deploy model**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-tour-deploy.png)

Next, the notebook sets up the SageMaker Model Monitor\.

## Monitor the Deployed Model<a name="studio-tour-monitor"></a>

Monitor the quality of your deployed models with Amazon SageMaker Model Monitor\. Model Monitor runs monitoring jobs on the endpoints where models are deployed\. You can use its built\-in monitoring capabilities, which don't require coding, or you can write code for custom analysis\. For more information, see [Amazon SageMaker Model Monitor](model-monitor.md)\.

The notebook first creates a processing job to generate baseline statistics\. Next, it creates a monitoring schedule, with a polling time of one hour, that compares the recent data captures to the baseline\.

**To monitor the deployed model**

1. Run all the code cells in the notebook sections titled **Host the model** and **Amazon SageMaker Model Monitor**\. This can take some time\. Note that this creates a different endpoint than the endpoint you created in the previous section\.

1. In Studio, choose the **SageMaker Endpoint List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Endpoint_squid.png) \)\.

1. Double\-click the endpoint \(named `demo-xgboost-customer-churn...`\) that was created by the notebook\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio_monitor_issues.png)

In the **Monitoring job history** list, you can see any issues that the monitoring jobs found\. To see details about an issue, choose the issue\.

**Important**  
You must shut down the kernel to stop monitoring\. To shut down the kernel, from the top Studio menu, choose **Kernel** then **Shut Down Kernel**\.

## Clean Up Resources<a name="studio-tour-cleanup"></a>

To stop incurring charges, you should clean up the resources that were created\.

To clean up the resources, follow the instructions in the notebook section titled **Clean up**\.