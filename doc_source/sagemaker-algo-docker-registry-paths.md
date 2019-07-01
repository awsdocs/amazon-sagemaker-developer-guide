# Common Parameters for Built\-In Algorithms<a name="sagemaker-algo-docker-registry-paths"></a>

The following table lists parameters for each of the algorithms provided by Amazon SageMaker\.


| Algorithm Name | Channel Name | Training Image and Inference Image Registry Path | Training Input Mode | File Type | Instance Class | Parallelizable | 
| --- | --- | --- | --- | --- | --- | --- | 
| BlazingText | train |  *<ecr\_path>*/blazingtext:*<tag>*  | File or Pipe | Text file \(one sentence per line with space\-separated tokens\)  | GPU \(single instance only\) or CPU | No | 
| DeepAR Forecasting | train and \(optionally\) test |  *<ecr\_path>*/forecasting\-deepar:*<tag>*  | File | JSON Lines or Parquet | GPU or CPU | Yes | 
| Factorization Machines | train and \(optionally\) test |  *<ecr\_path>*/factorization\-machines:*<tag>*  | File or Pipe | recordIO\-protobuf | CPU \(GPU for dense data\) | Yes | 
| Image Classification | train and validation, \(optionally\) train\_lst, validation\_lst, and model |  *<ecr\_path>*/image\-classification:*<tag>*  | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| IP Insights | train and \(optionally\) validation |  *<ecr\_path>*/ipinsights:*<tag>*  | File | CSV | CPU or GPU | Yes | 
| k\-means  | train and \(optionally\) test |  *<ecr\_path>*/kmeans:*<tag>*  | File or Pipe | recordIO\-protobuf or CSV | CPU or GPUCommon \(single GPU device on one or more instances\) | No | 
| k\-nearest\-neighbor \(k\-NN\) | train and \(optionally\) test |  *<ecr\_path>*/knn:*<tag>*  | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU \(single GPU device on one or more instances\) | Yes | 
|  LDA  | train and \(optionally\) test |  *<ecr\_path>*/lda:*<tag>*  | File or Pipe | recordIO\-protobuf or CSV | CPU \(single instance only\) | No | 
| Linear Learner | train and \(optionally\) validation, test, or both | <ecr\_path>/linear\-learner:<tag> | File or Pipe | recordIO\-protobuf or CSV | CPU or GPU | Yes | 
| Neural Topic Model | train and \(optionally\) validation, test, or both |  *<ecr\_path>*/ntm:*<tag>*  | File or Pipe | recordIO\-protobuf or CSV | GPU or CPU | Yes | 
| Object2Vec | train and \(optionally\) validation, test, or both |  *<ecr\_path>*/object2vec:*<tag>*  | File | JSON Lines  | GPU or CPU \(single instance only\) | No | 
| Object Detection | train and validation, \(optionally\) train\_annotation, validation\_annotation, and model |  *<ecr\_path>*/object\-detection:*<tag>*  | File or Pipe | recordIO or image files \(\.jpg or \.png\)  | GPU | Yes | 
| PCA | train and \(optionally\) test |  *<ecr\_path>*/pca:*<tag>*  | File or Pipe | recordIO\-protobuf or CSV | GPU or CPU | Yes | 
| Random Cut Forest | train and \(optionally\) test |  *<ecr\_path>*/randomcutforest:*<tag>*  | File or Pipe | recordIO\-protobuf or CSV | CPU | Yes | 
| Semantic Segmentation | train and validation, train\_annotation, validation\_annotation, and \(optionally\) label\_map and model |  *<ecr\_path>*/semantic\-segmentation:*<tag>*  | File or Pipe | image files | GPU \(single instance only\) | No | 
|  Seq2Seq Modeling  | train, validation, and vocab | <ecr\_path>/seq2seq:<tag> | File | recordIO\-protobuf | GPU \(single instance only\) | No | 
| XGBoost | train and \(optionally\) validation |  *<ecr\_path>*/xgboost:*<tag>*  | File | CSV or LibSVM | CPU | Yes | 

Algorithms that are *parallelizable* can be deployed on multiple compute instances for distributed training\. For the **Training Image and Inference Image Registry Path** column, use the `:1` version tag to ensure that you are using a stable version of the algorithm\. You can reliably host a model trained using an image with the `:1` tag on an inference image that has the `:1` tag\. Using the `:latest` tag in the registry path provides you with the most up\-to\-date version of the algorithm, but might cause problems with backward compatibility\. Avoid using the `:latest` tag for production purposes\.

For the **Training Image and Inference Image Registry Path** column, depending on algorithm and region use one of the following values for *<ecr\_path>\.*

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html)

Use the paths and training input mode as follows:
+ To create a training job \(with a request to the [CreateTrainingJob](API_CreateTrainingJob.md) API\), specify the Docker Registry path and the training input mode for the training image\. You create a training job to train a model using a specific dataset\. 

   
+ To create a model \(with a [CreateModel](API_CreateModel.md) request\), specify the Docker Registry path for the inference image\. Amazon SageMaker launches machine learning compute instances that are based on the endpoint configuration and deploys the model, which includes the artifacts \(the result of model training\)\.