# Amazon SageMaker Autopilot notebook output<a name="autopilot-automate-model-development-notebook-output"></a>

Amazon SageMaker Autopilot manages the key tasks in an automatic machine learning \(AutoML\) process\. They are implemented by Autopilot with an AutoML job\. The AutoML job creates two notebooks that describe the plan that Autopilot follows to generate candidate models\. A candidate model consists of a \(pipeline, algorithm\) pair\. First, there’s a data exploration notebook, that describes what Autopilot learned about the data that you provided\. Second, there’s a candidate generation notebook, which uses the information about the data to generate candidates\. 

You can run both notebooks in Amazon SageMaker or locally if you have installed the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. You can share the notebooks just like any other SageMaker Studio notebook\. The notebooks are created for you to conduct experiment\. For example, you could edit the following items in the notebooks:
+ the preprocessors used on the data 
+ the number of hyperparameter optimization \(HPO\) runs and their parallelism
+ the algorithms to try
+ the instance types used for the HPO jobs
+ the hyperparameter ranges

Modifications to the candidate generation notebook are encouraged to be used as a learning tool\. This capability allows you to learn about how the decisions made during the machine learning process impact the your results\. 

**Note**  
When you run the notebooks in your default instance you incur baseline costs, but when you execute HPO jobs from the candidate notebook, these jobs use additional compute resources that incur additional costs\. 

## Data exploration notebook<a name="data-exploration-notebook"></a>

A notebook is produced during the analysis phase of the AutoML job that helps you identify problems in your dataset\. It identifies specific areas for investigation in order to help you identify upstream problems with your data that may result in a suboptimal model\. 

## Candidate generation notebook<a name="candidate-generation-notebook"></a>

The candidate generation notebook contains each suggested preprocessing step, algorithm, and hyperparameter ranges\. If you chose just to produce the notebook and not to run the AutoML job, you can then decide which candidates are to be trained and tuned\. They optimize automatically and a final, best candidate will be identified\. If you ran the job directly without seeing the candidates first, then only best candidate is displayed when you open the notebook after the completion of the job\.