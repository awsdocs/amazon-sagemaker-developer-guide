# Tune Multiple Algorithms to Find the Best Model<a name="multiple-algorithm-hpo"></a>

 When you create a new hyperparameter optimization \(HPO\) job with Amazon SageMaker, you have the option of using the console or the API\. You provide one or more job specifications for the different algorithms you’re testing\. These are called training definitions\. Each training definition has a name, an algorithm source, metrics selection, an objective metric, and a configuration for a set of hyperparameter values\. It also has a data configuration for setting up the input data channels for the algorithm you choose, and a setting for the output data location\. You select the resources you want to use for the training run\. 

**Topics**
+ [Get Started](multiple-algorithm-hpo-get-started.md)
+ [Managing Hyperparameter Tuning Jobs](multiple-algorithm-hpo-manage-tuning-jobs.md)
+ [Create a new single or multi\-algorithm HPO tuning job](multiple-algorithm-hpo-create-tuning-jobs.md)