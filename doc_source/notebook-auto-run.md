# Notebook\-based Workflows<a name="notebook-auto-run"></a>

You can use Amazon SageMaker Studio fully\-managed notebooks to interactively build, train, and deploy machine learning models\. However, there are various scenarios in which you might want to run your notebook as a non\-interactive, scheduled job\. For example, you might want to create regular audit reports that analyze all training jobs run over a certain time frame and analyze the business value of deploying those models into production\. Or you might want to scale up a feature engineering job after testing the data transformation logic on a small subset of data\. Other common use cases include:
+ Scheduling jobs for model drift monitoring
+ Exploring the parameter space for better models

Studio provides fast and simple tools built from the existing Amazon EventBridge, SageMaker Training and SageMaker Pipelines services to help you schedule your notebook jobs interactively\. You don’t have to craft your own custom solution or enlist features from other services that may require additional overhead in time and costs to deploy\.

The Studio notebook scheduling feature initiates a non\-interactive job for you based on any schedule you choose\. You can launch a job on demand or according to a fixed schedule\. You can also run multiple notebooks in parallel, and parameterize cells in your notebooks to customize the input parameters to your notebook\.

**Prerequisites**

To schedule a notebook job, make sure you meet the following criteria:
+ Ensure your Jupyter notebook and any initialization or startup scripts are self\-contained with respect to code and software packages\. Otherwise, your non\-interactive job may incur errors\.
+ Review [Constraints and considerations](notebook-auto-run-constraints.md) to make sure you properly configured your Jupyter notebook, network settings, and container settings\.
+ Ensure your notebook can access needed external resources, such as Amazon EMR clusters\.
+ If you connect to an Amazon EMR cluster in your notebook and want to parameterize your Amazon EMR connection command, you must apply a workaround using environment variables to pass parameters\. For details, see [Connect to an Amazon EMR cluster](scheduled-notebook-connect-emr.md)\.
+ If you connect to an Amazon EMR cluster using Kerberos, LDAP, or HTTP Basic Auth authentication, you must use the AWS Secrets Manager to pass your security credentials to your Amazon EMR connection command\. For details, see [Connect to an Amazon EMR cluster](scheduled-notebook-connect-emr.md)\.
+ \(Optional\) If you want the UI to preload a script to run upon notebook startup, your admin must install it with a Lifecycle Configuration \(LCC\)\. For information about how to use a LCC script, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)\.