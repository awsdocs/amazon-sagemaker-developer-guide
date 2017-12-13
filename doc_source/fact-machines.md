# Factorization Machines<a name="fact-machines"></a>

A factorization machine is a general\-purpose supervised learning algorithm that you can use for both classification and regression tasks\. It is an extension of a linear model that is designed to parsimoniously capture interactions between features within high dimensional sparse datasets\. For example, in a click prediction system, the factorization machine model can capture click rate patterns observed when ads from a certain ad\-category are placed on pages from a certain page\-category\. Factorization machines are a good choice for tasks dealing with high dimensional sparse datasets, such as click prediction and item recommendation\.

**Note**  
The Amazon SageMaker implementation of factorization machines considers only pair\-wise \(2nd order\) interactions between features\.

## Input/Output Interface<a name="fm-inputoutput"></a>

The factorization machines algorithm can be run in either in binary classification mode or regression mode\. In both modes, a dataset can be provided to the test channel along with the train channel dataset\. In regression mode, the testing dataset is scored using RMSE \(Root Mean Square Error\)\. In binary classification mode, the test dataset is scored using Binary Cross Entropy \(Log Loss\), Accuracy \(at threshold=0\.5\) and F1 Score \(at threshold =0\.5\)\.

The factorization machines algorithm currently supports training only on the `recordIO-protobuf` format\. Because their use case is predominantly on sparse data, `CSV` is not a good candidate\.

For inference, factorization machines support the `application/json` and `x-recordio-protobuf` formats\. For binary classification models, both the score and the predicted label are returned\. For regression, just the score is returned\.

Please see example notebooks for more details on training and inference file formats\.

## EC2 Instance Recommendation<a name="fm-instances"></a>

We recommend training and inference with CPU instances for both sparse and dense datasets\. In some circumstances, training with GPUs on dense data might provide some benefit\.


+ [Input/Output Interface](#fm-inputoutput)
+ [EC2 Instance Recommendation](#fm-instances)
+ [How Factorization Machines Work](fact-machines-howitworks.md)
+ [Factorization Machines Hyperparameters](fact-machines-hyperparameters.md)
+ [Factorization Machine Response Formats](fm-in-formats.md)