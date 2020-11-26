# Sample RL Workflow Using Amazon SageMaker RL<a name="sagemaker-rl-workflow"></a>

The following example describes the steps for developing RL models using Amazon SageMaker RL\.

For complete code examples, see the sample notebooks at [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/reinforcement\-learning](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/reinforcement_learning)\.

1. **Formulate the RL problem**—First, formulate the business problem into an RL problem\. For example, auto scaling enables services to dynamically increase or decrease capacity depending on conditions that you define\. Currently, this requires setting up alarms, scaling policies, thresholds, and other manual steps\. To solve this with RL, we define the components of the Markov Decision Process:

   1. **Objective**—Scale instance capacity so that it matches the desired load profile\.

   1. **Environment**—A custom environment that includes the load profile\. It generates a simulated load with daily and weekly variations and occasional spikes\. The simulated system has a delay between when new resources are requested and when they become available for serving requests\.

   1. **State**—The current load, number of failed jobs, and number of active machines\.

   1. **Action**—Remove, add, or keep the same number of instances\.

   1. **Reward**—A positive reward for successful transactions and a high penalty for failing transactions beyond a specified threshold\.

1. **Define the RL environment**—The RL environment can be the real world where the RL agent interacts or a simulation of the real world\. You can connect open source and custom environments developed using Gym interfaces and commercial simulation environments such as MATLAB and Simulink\.

1. **Define the presets**—The presets configure the RL training jobs and define the hyperparameters for the RL algorithms\.

1. **Write the training code**—Write training code as a Python script and pass the script to a SageMaker training job\. In your training code, import the environment files and the preset files, and then define the `main()` function\.

1. **Train the RL Model**—Use the SageMaker `RLEstimator` in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to start an RL training job\. If you are using local mode, the training job runs on the notebook instance\. When you use SageMaker for training, you can select GPU or CPU instances\. Store the output from the training job in a local directory if you train in local mode, or on Amazon S3 if you use SageMaker training\.

   The `RLEstimator` requires the following information as parameters\. 

   1. The source directory where the environment, presets, and training code are uploaded\.

   1. The path to the training script\.

   1. The RL toolkit and deep learning framework you want to use\. This automatically resolves to the Amazon ECR path for the RL container\.

   1. The training parameters, such as the instance count, job name, and S3 path for output\.

   1. Metric definitions that you want to capture in your logs\. These can also be visualized in CloudWatch and in SageMaker notebooks\.

1. **Visualize training metrics and output**—After a training job that uses an RL model completes, you can view the metrics you defined in the training jobs in CloudWatch,\. You can also plot the metrics in a notebook by using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) analytics library\. Visualizing metrics helps you understand how the performance of the model as measured by the reward improves over time\.
**Note**  
If you train in local mode, you can't visualize metrics in CloudWatch\.

1. **Evaluate the model**—Checkpointed data from the previously trained models can be passed on for evaluation and inference in the checkpoint channel\. In local mode, use the local directory\. In SageMaker training mode, you need to upload the data to S3 first\.

1. **Deploy RL models**—Finally, deploy the trained model on an endpoint hosted on SageMaker containers or on an edge device by using AWS IoT Greengrass\.

For more information on RL with SageMaker, see [Using RL with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_rl.html)\.