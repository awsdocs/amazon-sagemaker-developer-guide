# Amazon SageMaker Autopilot notebooks generated to manage AutoML tasks<a name="autopilot-automate-model-development-notebook-output"></a>

Amazon SageMaker Autopilot manages the key tasks in an automatic machine learning \(AutoML\) process using an AutoML job\. 

The AutoML job creates three notebook\-based reports that describe the plan that Autopilot follows to generate candidate models\. A candidate model consists of a \(pipeline, algorithm\) pair\. First, there’s a **data exploration** notebook that describes what Autopilot learned about the data that you provided\. Second, there’s a **candidate definition** notebook, which uses the information about the data to generate candidates\. Third, a **model insights** report that can help detail the performance characteristics of the best model in the leaderboard of an Autopilot experiment\.

**Topics**
+ [Amazon SageMaker Autopilot Data exploration report](autopilot-data-exploration-report.md)
+ [Candidate definition notebook](autopilot-candidate-generation-notebook.md)

You can run these notebooks in Amazon SageMaker, or locally, if you have installed the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. You can share the notebooks just like any other SageMaker Studio notebook\. The notebooks are created for you to conduct experiments\. For example, you could edit the following items in the notebooks:
+ Preprocessors used on the data 
+ Amount of hyperparameter optimization \(HPO\) runs and their parallelism
+ Algorithms to try
+ Instance types used for the HPO jobs
+ Hyperparameter ranges

Modifications to the candidate definition notebook are encouraged as a learning tool\. With this capability, you learn how decisions made during the machine learning process impact your results\. 

**Note**  
When you run the notebooks in your default instance, you incur baseline costs\. However, when you run HPO jobs from the candidate notebook, these jobs use additional compute resources that incur additional costs\. 