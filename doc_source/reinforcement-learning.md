# Use Reinforcement Learning with Amazon SageMaker<a name="reinforcement-learning"></a>

Reinforcement learning \(RL\) combines fields such as computer science, neuroscience, and psychology to determine how to map situations to actions to maximize a numerical reward signal\. This notion of a reward signal in RL stems from neuroscience research into how the human brain makes decisions about which actions maximize reward and minimize punishment\. In most situations, humans are not given explicit instructions on which actions to take, but instead must learn both which actions yield the most immediate rewards, and how those actions influence future situations and consequences\. 

The problem of RL is formalized using Markov decision processes \(MDPs\) that originate from dynamical systems theory\. MDPs aim to capture high\-level details of a real problem that a learning agent encounters over some period of time in attempting to achieve some ultimate goal\. The learning agent should be able to determine the current state of its environment and identify possible actions that affect the learning agent’s current state\. Furthermore, the learning agent’s goals should correlate strongly to the state of the environment\. A solution to a problem formulated in this way is known as a reinforcement learning method\. 

## What are the differences between reinforcement, supervised, and unsupervised learning paradigms?<a name="rl-differences"></a>

Machine learning can be divided into three distinct learning paradigms: supervised, unsupervised, and reinforcement\.

In supervised learning, an external supervisor provides a training set of labeled examples\. Each example contains information about a situation, belongs to a category, and has a label identifying the category to which it belongs\. The goal of supervised learning is to generalize in order to predict correctly in situations that are not present in the training data\. 

In contrast, RL deals with interactive problems, making it infeasible to gather all possible examples of situations with correct labels that an agent might encounter\. This type of learning is most promising when an agent is able to accurately learn from its own experience and adjust accordingly\. 

In unsupervised learning, an agent learns by uncovering structure within unlabeled data\. While a RL agent might benefit from uncovering structure based on its experiences, the sole purpose of RL is to maximize a reward signal\. 

**Topics**
+ [What are the differences between reinforcement, supervised, and unsupervised learning paradigms?](#rl-differences)
+ [Why is Reinforcement Learning Important?](#rl-why)
+ [Markov Decision Process \(MDP\)](#rl-terms)
+ [Key Features of Amazon SageMaker RL](#sagemaker-rl)
+ [Reinforcement Learning Sample Notebooks](#sagemaker-rl-notebooks)
+ [Sample RL Workflow Using Amazon SageMaker RL](sagemaker-rl-workflow.md)
+ [RL Environments in Amazon SageMaker](sagemaker-rl-environments.md)
+ [Distributed Training with Amazon SageMaker RL](sagemaker-rl-distributed.md)
+ [Hyperparameter Tuning with Amazon SageMaker RL](sagemaker-rl-tuning.md)

## Why is Reinforcement Learning Important?<a name="rl-why"></a>

RL is well\-suited for solving large, complex problems, such as supply chain management, HVAC systems, industrial robotics, game artificial intelligence, dialog systems, and autonomous vehicles\. Because RL models learn by a continuous process of receiving rewards and punishments for every action taken by the agent, it is possible to train systems to make decisions under uncertainty and in dynamic environments\. 

## Markov Decision Process \(MDP\)<a name="rl-terms"></a>

RL is based on models called Markov Decision Processes \(MDPs\)\. An MDP consists of a series of time steps\. Each time step consists of the following:

Environment  
Defines the space in which the RL model operates\. This can be either a real\-world environment or a simulator\. For example, if you train a physical autonomous vehicle on a physical road, that would be a real\-world environment\. If you train a computer program that models an autonomous vehicle driving on a road, that would be a simulator\.

State  
Specifies all information about the environment and past steps that is relevant to the future\. For example, in an RL model in which a robot can move in any direction at any time step, the position of the robot at the current time step is the state, because if we know where the robot is, it isn't necessary to know the steps it took to get there\.

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
+ An RL toolkit\. An RL toolkit manages the interaction between the agent and the environment and provides a wide selection of state of the art RL algorithms\. SageMaker supports the Intel Coach and Ray RLlib toolkits\. For information about Intel Coach, see [https://nervanasystems\.github\.io/coach/](https://nervanasystems.github.io/coach/)\. For information about Ray RLlib, see [https://ray\.readthedocs\.io/en/latest/rllib\.html](https://ray.readthedocs.io/en/latest/rllib.html)\.
+ An RL environment\. You can use custom environments, open\-source environments, or commercial environments\. For information, see [RL Environments in Amazon SageMaker](sagemaker-rl-environments.md)\.

The following diagram shows the RL components that are supported in SageMaker RL\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-rl-support.png)

## Reinforcement Learning Sample Notebooks<a name="sagemaker-rl-notebooks"></a>

The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker reinforcement learning\.


****  

| **Notebook Title** | **Description** | 
| --- | --- | 
|  [How to Train Batch RL Policies?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_cartpole_batch_coach/rl_cartpole_batch_coach.ipynb)  |  This notebook shows how to use batch RL to train a new policy from an offline dataset\.  | 
|  [How to Solve the Cart\-pole Balancing Problem?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_cartpole_coach/rl_cartpole_coach_gymEnv.ipynb)  |  This notebook shows how to solve the cart\-pole balancing problem with RL\.   | 
|  [How to Solve the Knapsack Problem?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_knapsack_coach_custom/rl_knapsack_coach_customEnv.ipynb)  |  This notebook shows how to use RL to solve the knapsack problem, and how [SageMaker Managed Spot Training](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_managed_spot_cartpole_coach/rl_managed_spot_cartpole_coach_gymEnv.ipynb) can be used to run training at a lower cost\.   | 
|  [How to Solve the Mountain Car Problem?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/reinforcement_learning/rl_mountain_car_coach_gymEnv/rl_mountain_car_coach_gymEnv.ipynb)  |  This notebook shows how to solve the mountain car control problem with RL\.  | 