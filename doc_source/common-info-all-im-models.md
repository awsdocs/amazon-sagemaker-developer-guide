# Common Information About Built\-in Algorithms<a name="common-info-all-im-models"></a>

The following table lists parameters for each of the algorithms provided by Amazon SageMaker\.


| Algorithm name | Channel name | Training input mode | File type | Instance class | Parallelizable | 
| --- | --- | --- | --- | --- | --- | 
| AutoGluon\-Tabular | training and \(optionally\) validation | File | CSV | CPU or GPU \(single instance only\) | No | 
| BlazingText | train | File or Pipe | Text file \(one sentence per line with space\-separated tokens\)  | CPU or GPU \(single instance only\)  | No | 
| CatBoost | training and \(optionally\) validation | File | CSV | CPU \(single instance only\) | No | 
| DeepAR Forecasting | train and \(optionally\) test | File | JSON Lines or Parquet | CPU or GPU | Yes | 
| Factorization Machines | train and \(optionally\) test | File or Pipe | recordIO\-protobuf | CPU \(GPU for dense data\) | Yes | 
| Image Classification \- MXNet | train and validation, \(optionally\) train\_lst, validation\_lst, and model | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| Image Classification \- TensorFlow | training and validation | File | image files \(\.jpg, \.jpeg, or \.png\)  | CPU or GPU | Yes \(only across multiple GPUs on a single instance\) | 
| IP Insights | train and \(optionally\) validation | File | CSV | CPU or GPU | Yes | 
| K\-Means | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU or GPUCommon \(single GPU device on one or more instances\) | No | 
| K\-Nearest\-Neighbors \(k\-NN\) | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU \(single GPU device on one or more instances\) | Yes | 
| LDA | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU \(single instance only\) | No | 
| LightGBM | train/training and \(optionally\) validation | File | CSV | CPU | Yes | 
| Linear Learner | train and \(optionally\) validation, test, or both | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU | Yes | 
| Neural Topic Model | train and \(optionally\) validation, test, or both | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU | Yes | 
| Object2Vec | train and \(optionally\) validation, test, or both | File | JSON Lines  | CPU or GPU \(single instance only\) | No | 
| Object Detection \- MXNet | train and validation, \(optionally\) train\_annotation, validation\_annotation, and model | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| Object Detection \- TensorFlow | training and validation | File | image files \(\.jpg, \.jpeg, or \.png\)  | GPU | Yes \(only across multiple GPUs on a single instance\) | 
| PCA | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU | Yes | 
| Random Cut Forest | train and \(optionally\) test | File or Pipe | recordIO\-protobuf or CSV | CPU | Yes | 
| Semantic Segmentation | train and validation, train\_annotation, validation\_annotation, and \(optionally\) label\_map and model | File or Pipe | Image files | GPU \(single instance only\) | No | 
| Seq2Seq Modeling | train, validation, and vocab | File | recordIO\-protobuf | GPU \(single instance only\) | No | 
| TabTransformer | training and \(optionally\) validation | File | CSV | CPU or GPU \(single instance only\) | No | 
| Text Classification \- TensorFlow | training and validation | File | CSV | CPU or GPU | Yes \(only across multiple GPUs on a single instance\) | 
| XGBoost \(0\.90\-1, 0\.90\-2, 1\.0\-1, 1\.2\-1, 1\.2\-21\) | train and \(optionally\) validation | File or Pipe | CSV, LibSVM, or Parquet | CPU \(or GPU for 1\.2\-1\) | Yes | 

Algorithms that are *parallelizable* can be deployed on multiple compute instances for distributed training\.

The following topics provide information about data formats, recommended Amazon EC2 instance types, and CloudWatch logs common to all of the built\-in algorithms provided by Amazon SageMaker\.

**Tip**  
To look up the Docker image URIs of the built\-in algorithms managed by SageMaker, see [Docker Registry Paths and Example Code](sagemaker-algo-docker-registry-paths.md)\. 

**Topics**
+ [Common Data Formats for Built\-in Algorithms](sagemaker-algo-common-data-formats.md)
+ [Instance Types for Built\-in Algorithms](cmn-info-instance-types.md)
+ [Logs for Built\-in Algorithms](common-info-all-sagemaker-models-logs.md)