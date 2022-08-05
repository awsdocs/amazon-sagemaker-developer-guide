# Built\-in SageMaker Algorithms for Tabular Data<a name="algorithms-tabular"></a>

Amazon SageMaker provides built\-in algorithms that are tailored to the analysis of tabular data\. The built\-in SageMaker algorithms for tabular data can be used for either classification or regression problems\.
+ [AutoGluon\-Tabular](autogluon-tabular.md)—an open\-source AutoML framework that succeeds by ensembling models and stacking them in multiple layers\. 
+ [CatBoost](catboost.md)—an implementation of the gradient\-boosted trees algorithm that introduces ordered boosting and an innovative algorithm for processing categorical features\.
+ [Factorization Machines Algorithm](fact-machines.md)—an extension of a linear model that is designed to economically capture interactions between features within high\-dimensional sparse datasets\.
+ [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md)—a non\-parametric method that uses the k nearest labeled points to assign a label to a new data point for classification or a predicted target value from the average of the k nearest points for regression\.
+ [LightGBM](lightgbm.md)—an implementation of the gradient\-boosted trees algorithm that adds two novel techniques for improved efficiency and scalability: Gradient\-based One\-Side Sampling \(GOSS\) and Exclusive Feature Bundling \(EFB\)\.
+ [Linear Learner Algorithm](linear-learner.md)—learns a linear function for regression or a linear threshold function for classification\.
+ [TabTransformer](tabtransformer.md)—a novel deep tabular data modeling architecture built on self\-attention\-based Transformers\. 
+ [XGBoost Algorithm](xgboost.md)—an implementation of the gradient\-boosted trees algorithm that combines an ensemble of estimates from a set of simpler and weaker models\.


| Algorithm name | Channel name | Training input mode | File type | Instance class | Parallelizable | 
| --- | --- | --- | --- | --- | --- | 
| AutoGluon\-Tabular | train and \(optionally\) validation | File | CSV | CPU or GPU \(single instance only\) | No | 
| CatBoost | train and \(optionally\) validation | File | CSV | CPU \(single instance only\) | No | 
| Factorization Machines | train and \(optionally\) test | File or Pipe | recordIO\-protobuf | CPU \(GPU for dense data\) | Yes | 
| K\-Nearest\-Neighbors \(k\-NN\) | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU \(single GPU device on one or more instances\) | Yes | 
| LightGBM | train and \(optionally\) validation | File | CSV | CPU \(single instance only\) | No | 
| Linear Learner | train and \(optionally\) validation, test, or both | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU | Yes | 
| TabTransformer | train and \(optionally\) validation | File | CSV | CPU or GPU \(single instance only\) | No | 
| XGBoost \(0\.90\-1, 0\.90\-2, 1\.0\-1, 1\.2\-1, 1\.2\-21\) | train and \(optionally\) validation | File or Pipe | CSV, LibSVM, or Parquet | CPU \(or GPU for 1\.2\-1\) | Yes | 