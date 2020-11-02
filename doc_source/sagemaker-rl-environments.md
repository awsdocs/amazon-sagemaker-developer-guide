# RL Environments in Amazon SageMaker<a name="sagemaker-rl-environments"></a>

Amazon SageMaker RL uses environments to mimic real\-world scenarios\. Given the current state of the environment and an action taken by the agent or agents, the simulator processes the impact of the action, and returns the next state and a reward\. Simulators are useful in cases where it is not safe to train an agent in the real world \(for example, flying a drone\) or if the RL algorithm takes a long time to converge \(for example, when playing chess\)\.

The following diagram shows an example of the interactions with a simulator for a car racing game\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-rl-flow.png)

The simulation environment consists of an agent and a simulator\. Here, a convolutional neural network \(CNN\) consumes images from the simulator and generates actions to control the game controller\. With multiple simulations, this environment generates training data of the form `state_t`, `action`, `state_t+1`, and `reward_t+1`\. Defining the reward is not trivial and impacts the RL model quality\. We want to provide a few examples of reward functions, but would like to make it user\-configurable\. 

**Topics**
+ [Use OpenAI Gym Interface for Environments in SageMaker RL](#sagemaker-rl-environments-gym)
+ [Use Open\-Source Environments](#sagemaker-rl-environments-open)
+ [Use Commercial Environments](#sagemaker-rl-environments-commercial)

## Use OpenAI Gym Interface for Environments in SageMaker RL<a name="sagemaker-rl-environments-gym"></a>

To use OpenAI Gym environments in SageMaker RL, use the following API elements\. For more information about OpenAI Gym, see [https://gym\.openai\.com/docs/](https://gym.openai.com/docs/)\.
+ `env.action_space`—Defines the actions the agent can take, specifies whether each action is continuous or discrete, and specifies the minimum and maximum if the action is continuous\.
+ `env.observation_space`—Defines the observations the agent receives from the environment, as well as minimum and maximum for continuous observations\.
+ `env.reset()`—Initializes a training episode\. The `reset()` function returns the initial state of the environment, and the agent uses the initial state to take its first action\. The action is then sent to `step()` repeatedly until the episode reaches a terminal state\. When `step()` returns `done = True`, the episode ends\. The RL toolkit re\-initializes the environment by calling `reset()`\.
+ `step()`—Takes the agent action as input and outputs the next state of the environment, the reward, whether the episode has terminated, and an `info` dictionary to communicate debugging information\. It is the responsibility of the environment to validate the inputs\.
+ `env.render()`—Used for environments that have visualization\. The RL toolkit calls this function to capture visualizations of the environment after each call to the `step()` function\.

## Use Open\-Source Environments<a name="sagemaker-rl-environments-open"></a>

You can use open\-source environments, such as EnergyPlus and RoboSchool, in SageMaker RL by building your own container\. For more information about EnergyPlus, see [https://energyplus\.net/](https://energyplus.net/)\. For more information about RoboSchool, see [https://github\.com/openai/roboschool](https://github.com/openai/roboschool)\. The HVAC and RoboSchool examples in the [SageMaker examples repository](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/reinforcement_learning) show how to build a custom container to use with SageMaker RL:

## Use Commercial Environments<a name="sagemaker-rl-environments-commercial"></a>

You can use commercial environments, such as MATLAB and Simulink, in SageMaker RL by building your own container\. You need to manage your own licenses\.