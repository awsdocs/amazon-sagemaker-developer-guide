# Unsupervised Built\-in SageMaker Algorithms<a name="algorithms-unsupervised"></a>

Amazon SageMaker provides several built\-in algorithms that can be used for a variety of unsupervised learning tasks such as clustering, dimension reduction, pattern recognition, and anomaly detection\.
+ [IP Insights](ip-insights.md)—learns the usage patterns for IPv4 addresses\. It is designed to capture associations between IPv4 addresses and various entities, such as user IDs or account numbers\.
+ [K\-Means Algorithm](k-means.md)—finds discrete groupings within data, where members of a group are as similar as possible to one another and as different as possible from members of other groups\.
+ [Principal Component Analysis \(PCA\) Algorithm](pca.md)—reduces the dimensionality \(number of features\) within a dataset by projecting data points onto the first few principal components\. The objective is to retain as much information or variation as possible\. For mathematicians, principal components are eigenvectors of the data's covariance matrix\.
+ [Random Cut Forest \(RCF\) Algorithm](randomcutforest.md)—detects anomalous data points within a data set that diverge from otherwise well\-structured or patterned data\.


| Algorithm name | Channel name | Training input mode | File type | Instance class | Parallelizable | 
| --- | --- | --- | --- | --- | --- | 
| IP Insights | train and \(optionally\) validation | File | CSV | CPU or GPU | Yes | 
| K\-Means | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU or GPUCommon \(single GPU device on one or more instances\) | No | 
| PCA | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | GPU or CPU | Yes | 
| Random Cut Forest | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU | Yes | 