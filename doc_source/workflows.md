# SageMaker Workflows<a name="workflows"></a>

As you scale your machine learning \(ML\) operations, you can use Amazon SageMaker fully managed workflow services to implement continuous integration and deployment \(CI/CD\) practices for your ML lifecycle\. With the SageMaker Pipelines SDK, you choose and integrate pipeline steps into a unified solution that automates the model\-building process from data preparation to model deployment\. While SageMaker Pipelines manages training and inference, SageMaker Lineage Tracking tracks the history of your data in the pipeline run and saves artifacts for compliance verification\. SageMaker Pipelines and Lineage Tracking form a holistic framework to help you quickly deliver high\-performing models to production\.

SageMaker provides additional tooling to support your workflow\. If you don't want to build your pipeline from scratch, use pre\-built ML pipeline templates provided by SageMaker Projects\. You supply the training and deployment code while SageMaker manages the infrastructure and orchestrates the pipeline run\. If you want to schedule non\-interactive batch runs of your Jupyter notebook, use the notebook\-based workflows service to initiate standalone or regular runs on a schedule you define\. Lastly, SageMaker provides operators if you choose to build and manage your pipelines on Kubernetes clusters\.

In summary, SageMaker offers the following workflow technologies:
+ [Amazon SageMaker Model Building Pipelines](pipelines.md): Tool for building and managing ML pipelines\.
+ [Track the Lineage of a SageMaker ML Pipeline](pipelines-lineage-tracking.md): Tool for artifact tracking to aid auditing and compliance verification\.
+ [Automate MLOps with SageMaker Projects](sagemaker-projects.md): Pre\-built templates you can use to create your pipelines, depending upon the steps you need\.
+ [Kubernetes Orchestration](kubernetes-workflows.md): SageMaker custom operators for your Kubernetes cluster and components for Kubeflow Pipelines\.
+ [Notebook\-based Workflows](notebook-auto-run.md): On demand or scheduled non\-interactive batch runs of your Jupyter notebook\.

You can also leverage other services that integrate with SageMaker to build your workflow\. Options include the following services:
+ [Airflow Workflows](https://sagemaker.readthedocs.io/en/stable/workflows/airflow/index.html): SageMaker APIs to export configurations for creating and managing Airflow workflows\.
+ [AWS Step Functions](https://sagemaker.readthedocs.io/en/stable/workflows/step_functions/index.html): Multi\-step ML workflows in Python that orchestrate SageMaker infrastructure without having to provision your resources separately\.

For more information on managing SageMaker training and inference, see [Amazon SageMaker Python SDK Workflows](https://sagemaker.readthedocs.io/en/stable/workflows/index.html)\.

**Topics**
+ [Amazon SageMaker Model Building Pipelines](pipelines.md)
+ [Automate MLOps with SageMaker Projects](sagemaker-projects.md)
+ [Amazon SageMaker ML Lineage Tracking](lineage-tracking.md)
+ [Kubernetes Orchestration](kubernetes-workflows.md)
+ [Notebook\-based Workflows](notebook-auto-run.md)
+ [Amazon SageMaker Workflows FAQ](mlopsfaq.md)