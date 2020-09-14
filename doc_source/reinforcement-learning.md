# Use Reinforcement Learning with Amazon SageMaker<a name="reinforcement-learning"></a>

Reinforcement learning \(RL\) is a machine learning technique that attempts to learn a strategy, called a policy, that optimizes an objective for an agent acting in an environment\. For example, the agent might be a robot, the environment might be a maze, and the goal might be to successfully navigate the maze in the smallest amount of time\. In RL, the agent takes an action, observes the state of the environment, and gets a reward based on the value of the current state of the environment\. The goal is to maximize the long\-term reward that the agent receives as a result of its actions\. RL is well\-suited for solving problems where an agent can make autonomous decisions\.

**Topics**
+ [Why is Reinforcement Learning Important?](#rl-why)
+ [Markov Decision Process \(MDP\)](#rl-terms)
+ [Key Features of Amazon SageMaker RL](#sagemaker-rl)
+ [Reinforcement Learning Sample Notebooks](#sagemaker-rl-notebooks)
+ [Sample RL Workflow Using Amazon SageMaker RL](sagemaker-rl-workflow.md)
+ [RL Environments in Amazon SageMaker](sagemaker-rl-environments.md)
+ [Distributed Training with Amazon SageMaker RL](sagemaker-rl-distributed.md)
+ [Hyperparameter Tuning with Amazon SageMaker RL](sagemaker-rl-tuning.md)

## Why is Reinforcement Learning Important?<a name="rl-why"></a>

RL is well\-suited for solving large, complex problems\. For example, supply chain management, HVAC systems, industrial robotics, game artificial intelligence, dialog systems, and autonomous vehicles\. Because RL models learn by a continuous process of receiving rewards and punishments for every action taken by the agent, it is possible to train systems to make decisions under uncertainty and in dynamic environments\.

## Markov Decision Process \(MDP\)<a name="rl-terms"></a>

RL is based on models called Markov Decision Processes \(MDPs\)\. An MDP consists of a series of time steps\. Each time step consists of the following:

Environment  
Defines the space in which the RL model operates\. This can be either a real\-world environment or a simulator\. For example, if you train a physical autonomous vehicle on a physical road, that would be a real\-world environment\. If you train a computer program that models an autonomous vehicle driving on a road, that would be a simulator\.

State  
Specifies all information about the environment and past steps that is relevant to the future\. For example, in an RL model in which a robot can move in any direction at any time step, then the position of the robot at the current time step is the state, because if we know where the robot is, it isn't necessary to know the steps it took to get there\.

Action  
What the agent does\. For example, the robot takes a step forward\.

Reward  
A number that represents the value of the state that resulted from the last action that the agent took\. For example, if the goal is for a robot to find treasure, the reward for finding treasure might be 5, and the reward for not finding treasure might be 0\. The RL model attempts to find a strategy that optimizes the cumulative reward over the long term\. This strategy is called a *policy*\.

Observation  
Information about the state of the environment that is available to the agent at each step\. This might be the entire state, or it might be just a part of the state\. For example, the agent in a chess\-playing model would be able to observe the entire state of the board at any step, but a robot in a maze might only be able to observe a small portion of the maze that it currently occupies\.

Typically, training in RL consists of many *episodes*\. An episode consists of all of the time steps in an MDP from the initial state until the environment reaches the terminal state\.

## Key Features of Amazon SageMaker RL<a name="sagemaker-rl"></a>

To train RL models in SageMaker RL, use the following components: 
+ A deep learning \(DL\) framework\. Currently, SageMaker supports RL in TensorFlow and Apache MXNet\.
+ An RL toolkit\. An RL toolkit manages the interaction between the agent and the environment, and provides a wide selection of state of the art RL algorithms\. SageMaker supports the Intel Coach and Ray RLlib toolkits\. For information about Intel Coach, see [https://nervanasystems\.github\.io/coach/](https://nervanasystems.github.io/coach/)\. For information about Ray RLlib, see [https://ray\.readthedocs\.io/en/latest/rllib\.html](https://ray.readthedocs.io/en/latest/rllib.html)\.
+ An RL environment\. You can use custom environments, open\-source environments, or commercial environments\. For information, see [RL Environments in Amazon SageMaker](sagemaker-rl-environments.md)\.

The following diagram shows the RL components that are supported in SageMaker RL\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-rl-support.png)

## Reinforcement Learning Sample Notebooks<a name="sagemaker-rl-notebooks"></a>

 The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker reinforcement learning\. 


****  

| **Title** | **Description** | 
| --- | --- | 
|  [How to Train Batch RL Policies?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_cartpole_batch_coach/rl_cartpole_batch_coach.ipynb)  |   We demonstrate how to use batch RL to train a new policy from an offline dataset\.  | 
|  [How to Solve the Cart\-pole Balancing Problem?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_cartpole_coach/rl_cartpole_coach_gymEnv.ipynb)  |  We demonstrate how to solve the Cart\-pole balancing problem with RL\.   | 
|  [How to Solve the Knapsack Problem?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_knapsack_coach_custom/rl_knapsack_coach_customEnv.ipynb)  |   We demonstrate how to use RL to solve the Knapsack problem, and how [SageMaker Managed Spot Training](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_managed_spot_cartpole_coach/rl_managed_spot_cartpole_coach_gymEnv.ipynb) can be used to run training at a lower cost\.   | 
|  [How to Solve the Mountain Car Problem?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_mountain_car_coach_gymEnv/rl_mountain_car_coach_gymEnv.ipynb)  |  We demonstrate how to solve the Mountain Car control problem with RL\.  | 
|  [How to Train a Distributed Object Tracker with RL?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_objecttracker_robomaker_coach_gazebo/rl_objecttracker_coach_robomaker.ipynb)  |  Using Robomaker we demonstrate how to train a Distributed Object Tracker that learns to track and follow another Robot\.  | 