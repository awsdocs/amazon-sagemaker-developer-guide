# Amazon SageMaker Autopilot notebooks generated to manage AutoML tasks<a name="autopilot-automate-model-development-notebook-output"></a>

Amazon SageMaker Autopilot manages the key tasks in an automatic machine learning \(AutoML\) process\. They are implemented by Autopilot with an AutoML job\. The AutoML job creates two notebooks that describe the plan that Autopilot follows to generate candidate models\. A candidate model consists of a \(pipeline, algorithm\) pair\. First, there’s a data exploration notebook, that describes what Autopilot learned about the data that you provided\. Second, there’s a candidate definition notebook, which uses the information about the data to generate candidates\. 

**Topics**
+ [Amazon SageMaker Autopilot Data exploration report](autopilot-data-exploration-report.md)
+ [Candidate definition notebook](autopilot-candidate-generation-notebook.md)

You can run both notebooks in Amazon SageMaker or locally if you have installed the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. You can share the notebooks just like any other SageMaker Studio notebook\. The notebooks are created for you to conduct experiment\. For example, you could edit the following items in the notebooks:
+ the preprocessors used on the data 
+ the number of hyperparameter optimization \(HPO\) runs and their parallelism
+ the algorithms to try
+ the instance types used for the HPO jobs
+ the hyperparameter ranges

Modifications to the candidate definition notebook are encouraged to be used as a learning tool\. This capability allows you to learn about how the decisions made during the machine learning process impact your results\. 

**Note**  
When you run the notebooks in your default instance you incur baseline costs, but when you execute HPO jobs from the candidate notebook, these jobs use additional compute resources that incur additional costs\. 