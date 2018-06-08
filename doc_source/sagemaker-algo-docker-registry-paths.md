# Algorithms Provided by Amazon SageMaker: Common Parameters<a name="sagemaker-algo-docker-registry-paths"></a>

The following table lists parameters for each of the algorithms provided by Amazon SageMaker\.


| Algorithm Name | Channel Name | Training Image and Inference Image Registry Path | Training Input Mode | File Type | Instance Class | 
| --- | --- | --- | --- | --- | --- | 
| k\-means  | train and \(optionally\) test |  *<ecr\_path>*/kmeans:*<tag>*  |  File or Pipe  | recordIO\-protobuf or CSV | CPU or GPU \(single GPU device on one or more instances\) | 
| PCA | train and \(optionally\) test |  *<ecr\_path>*/pca:*<tag>*  |  File or Pipe  | recordIO\-protobuf or CSV | GPU or CPU | 
|  LDA  | train and \(optionally\) test |  *<ecr\_path>*/lda:*<tag>*  |  File or Pipe  | recordIO\-protobuf or CSV | CPU \(single instance only\) | 
| Factorization Machines | train and \(optionally\) test |  *<ecr\_path>*/factorization\-machines:*<tag>*  |  File or Pipe  | recordIO\-protobuf | CPU \(GPU for dense data\) | 
| Linear Learner | train and \(optionally\) validation, test, or both | <ecr\_path>/linear\-learner:<tag> |  File or Pipe  | recordIO\-protobuf or CSV | CPU or GPU | 
| Neural Topic Model | train and \(optionally\) validation, test, or both |  *<ecr\_path>*/ntm:*<tag>*  |  File or Pipe  | recordIO\-protobuf or CSV | GPU or CPU | 
| Random Cut Forest | train and \(optionally\) test |  *<ecr\_path>*/randomcutforest:*<tag>*  |  File or Pipe  | recordIO\-protobuf or CSV | CPU | 
|  Seq2Seq Modeling  | train, validation, and vocab | <ecr\_path>/seq2seq:<tag> |  File  | recordIO\-protobuf | GPU \(single instance only\) | 
| XGBoost | train and \(optionally\) validation |  *<ecr\_path>*/xgboost:*<tag>*  |  File  | CSV or LibSVM | CPU | 
| Image Classification | train and validation, \(optionally\) train\_lst and validation\_lst |  *<ecr\_path>*/image\-classification:*<tag>*  |  File  | recordIO or image files \(\.jpg or \.png\)  | GPU | 
| DeepAR Forecasting | train and \(optionally\) test |  *<ecr\_path>*/forecasting\-deepar:*<tag>*  |  File  | JSON Lines or Parquet | GPU or CPU | 
| BlazingText | train |  *<ecr\_path>*/blazingtext:*<tag>*  |  File  | Text file \(one sentence per line with with space\-separated tokens\)  | GPU \(single instance only\) or CPU | 

For the **Training Image and Inference Image Registry Path** column, use the `:1` version tag to ensure that you are using a stable version of the algorithm\. You can reliably host a model trained using an image with the `:1` tag on an inference image that has the `:1` tag\. Using the `:latest` tag in the registry path provides you with the most up\-to\-date version of the algorithm, but might cause problems with backward compatibility\. Avoid using the `:latest` tag for production purposes\.

For the **Training Image and Inference Image Registry Path** column, depending on algorithm and region use one of the following values for *<ecr\_path>\.*

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html)

Use the paths and training input mode as follows:
+ To create a training job \(with a request to the [CreateTrainingJob](API_CreateTrainingJob.md) API\), specify the Docker Registry path and the training input mode for the training image\. You create a training job to train a model using a specific dataset\. 

   
+ To create a model \(with a [CreateModel](API_CreateModel.md) request\), specify the Docker Registry path for the inference image\. Amazon SageMaker launches machine learning compute instances that are based on the endpoint configuration and deploys the model, which includes the artifacts \(the result of model training\)\.