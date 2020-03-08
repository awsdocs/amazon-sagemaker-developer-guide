# SageMaker Autopilot Notebook Output<a name="autopilot-automate-model-development-notebook-output"></a>

 During the analysis phase of the AutoML job, two notebooks are created that describe the plan that Autopilot will follow to generate candidate models\. First, there’s a data exploration notebook, that describes what Autopilot learned about the data that you provided\. Second, there’s a candidate generation notebook, which uses the information about the data to generate candidates\. Together, these notebooks describe the plan that would be executed on autopilot if you don’t choose to stop with candidate generation only\. 

 You can run both notebooks in SageMaker or locally if you have installed the SageMaker Python SDK\. You can share the notebooks just like any other SageMaker Studio notebook\. Modifications on the candidate generation notebook is encouraged\. The notebooks are created for you to experiment\. When you run the notebooks in your default instance you will incur baseline costs, but when you execute HPO jobs from the candidate notebook, these jobs use additional compute resources that will incur additional costs\. 

## Candidate Generation Notebook<a name="candidate-generation-notebook"></a>

 The candidate generation notebook contains each suggested preprocessing step, suggested algorithm, and suggested hyperparameter ranges\. If you chose to only produce the notebook and not run the AutoML job, you can then decide which candidates to be trained and tuned\. They optimize automatically and a final, best candidate will be identified\. If you ran the job directly without seeing the candidates first, the best candidate is displayed when you open the notebook after the job’s completion\. 

## Data Exploration Notebook<a name="data-exploration-notebook"></a>

 A second notebook is produced during the analysis phase of the AutoML job that helps you identify problems in your dataset\. It identifies specific areas for investigation in order to help you identify upstream problems with your data that may result in a suboptimal model\. 