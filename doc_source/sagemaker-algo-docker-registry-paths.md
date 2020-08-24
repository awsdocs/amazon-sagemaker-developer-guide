# Common parameters for built\-in algorithms<a name="sagemaker-algo-docker-registry-paths"></a>

The following table lists parameters for each of the algorithms provided by Amazon SageMaker\.


| Algorithm name | Channel name | Training image and inference image registry path | Training input mode | File type | Instance class | Parallelizable | 
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
| XGBoost \(0\.90\-1, 0\.90\-2, 1\.0\-1\) | train and \(optionally\) validation |  *<ecr\_path>*/sagemaker\-xgboost:*1\.0\-1\-cpu\-py3*  | File or Pipe | CSV, LibSVM, recordIO\-protobuf, or Parquet | CPU | Yes | 

**Note**  
For XGBoost, do not use `:latest` or `:1`\. Use the specific version you require such as, `:0.90-1-cpu-py3`, `:0.90-2-cpu-py3`, or `:1.0-1-cpu-py3`\.

Algorithms that are *parallelizable* can be deployed on multiple compute instances for distributed training\. For the **Training Image and Inference Image Registry Path** column, use the `:1` version tag to ensure that you are using a stable version of the algorithm\. You can reliably host a model trained using an image with the `:1` tag on an inference image that has the `:1` tag\. Using the `:latest` tag in the registry path provides you with the most up\-to\-date version of the algorithm, but might cause problems with backward compatibility\. Avoid using the `:latest` tag for production purposes\.

For the **Training Image and Inference Image Registry Path** column, depending on algorithm and region use one of the following values for *<ecr\_path>\.*

**Algorithms**: BlazingText, Image Classification, Object Detection, Semantic Segmentation, and Seq2Seq


| AWS Region | Training image and inference image registry path | 
| --- | --- | 
| us\-west\-1 | 632365934929\.dkr\.ecr\.us\-west\-1\.amazonaws\.com | 
| us\-west\-2 | 433757028032\.dkr\.ecr\.us\-west\-2\.amazonaws\.com | 
| us\-east\-1 | 811284229777\.dkr\.ecr\.us\-east\-1\.amazonaws\.com | 
| us\-east\-2 | 825641698319\.dkr\.ecr\.us\-east\-2\.amazonaws\.com | 
| ap\-east\-1 | 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com | 
| ap\-northeast\-1 | 501404015308\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com | 
| ap\-northeast\-2 | 306986355934\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com | 
| ap\-south\-1 | 991648021394\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com | 
| ap\-southeast\-1 | 475088953585\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com | 
| ap\-southeast\-2 | 544295431143\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com | 
| ca\-central\-1 | 469771592824\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com | 
| cn\-north\-1 | 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn | 
| cn\-northwest\-1 | 387376663083\.dkr\.ecr\.cn\-northwest\-1\.amazonaws\.com\.cn | 
| eu\-central\-1 | 813361260812\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com | 
| eu\-north\-1 | 669576153137\.dkr\.ecr\.eu\-north\-1\.amazonaws\.com | 
| eu\-west\-1 | 685385470294\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com | 
| eu\-west\-2 | 644912444149\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com | 
| eu\-west\-3 | 749696950732\.dkr\.ecr\.eu\-west\-3\.amazonaws\.com | 
| me\-south\-1 | 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com | 
| sa\-east\-1 | 855470959533\.dkr\.ecr\.sa\-east\-1\.amazonaws\.com | 
| us\-gov\-west\-1 | 226302683700\.dkr\.ecr\.us\-gov\-west\-1\.amazonaws\.com | 

**Algorithms**: DeepAR Forecasting


| AWS Region | Training image and inference image registry path | 
| --- | --- | 
| us\-west\-1 | 632365934929\.dkr\.ecr\.us\-west\-1\.amazonaws\.com | 
| us\-west\-2 | 156387875391\.dkr\.ecr\.us\-west\-2\.amazonaws\.com | 
| us\-east\-1 | 522234722520\.dkr\.ecr\.us\-east\-1\.amazonaws\.com | 
| us\-east\-2 | 566113047672\.dkr\.ecr\.us\-east\-2\.amazonaws\.com | 
| ap\-east\-1 | 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com | 
| ap\-northeast\-1 | 633353088612\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com | 
| ap\-northeast\-2 | 204372634319\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com | 
| ap\-south\-1 | 991648021394\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com | 
| ap\-southeast\-1 | 475088953585\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com | 
| ap\-southeast\-2 | 514117268639\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com | 
| ca\-central\-1 | 469771592824\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com | 
| cn\-north\-1 | 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn | 
| cn\-northwest\-1 | 387376663083\.dkr\.ecr\.cn\-northwest\-1\.amazonaws\.com\.cn | 
| eu\-north\-1 | 669576153137\.dkr\.ecr\.eu\-north\-1\.amazonaws\.com | 
| eu\-central\-1 | 495149712605\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com | 
| eu\-west\-1 | 224300973850\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com | 
| eu\-west\-2 | 644912444149\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com | 
| eu\-west\-3 | 749696950732\.dkr\.ecr\.eu\-west\-3\.amazonaws\.com | 
| me\-south\-1 | 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com | 
| sa\-east\-1 | 855470959533\.dkr\.ecr\.sa\-east\-1\.amazonaws\.com | 
| us\-gov\-west\-1 | 226302683700\.dkr\.ecr\.us\-gov\-west\-1\.amazonaws\.com | 

**Algorithms**: Factorization Machines, IP Insights, k\-means, k\-nearest\-neighbor, Linear Learner, Object2Vec, Neural Topic Model,PCA, and Random Cut Forest


| AWS Region | Training image and inference image registry path | 
| --- | --- | 
| us\-west\-1 | 632365934929\.dkr\.ecr\.us\-west\-1\.amazonaws\.com | 
| us\-west\-2 | 174872318107\.dkr\.ecr\.us\-west\-2\.amazonaws\.com | 
| us\-east\-1 | 382416733822\.dkr\.ecr\.us\-east\-1\.amazonaws\.com | 
| us\-east\-2 | 404615174143\.dkr\.ecr\.us\-east\-2\.amazonaws\.com | 
| ap\-east\-1 | 286214385809\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com | 
| ap\-northeast\-1 | 351501993468\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com | 
| ap\-northeast\-2 | 835164637446\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com | 
| ap\-south\-1 | 991648021394\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com | 
| ap\-southeast\-1 | 475088953585\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com | 
| ap\-southeast\-2 | 712309505854\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com | 
| ca\-central\-1 | 469771592824\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com | 
| cn\-north\-1 | 390948362332\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn | 
| cn\-northwest\-1 | 387376663083\.dkr\.ecr\.cn\-northwest\-1\.amazonaws\.com\.cn | 
| eu\-central\-1 | 664544806723\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com | 
| eu\-north\-1 | 669576153137\.dkr\.ecr\.eu\-north\-1\.amazonaws\.com | 
| eu\-west\-1 | 438346466558\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com | 
| eu\-west\-2 | 644912444149\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com | 
| eu\-west\-3 | 749696950732\.dkr\.ecr\.eu\-west\-3\.amazonaws\.com | 
| me\-south\-1 | 249704162688\.dkr\.ecr\.me\-south\-1\.amazonaws\.com | 
| sa\-east\-1 | 855470959533\.dkr\.ecr\.sa\-east\-1\.amazonaws\.com | 
| us\-gov\-west\-1 | 226302683700\.dkr\.ecr\.us\-gov\-west\-1\.amazonaws\.com | 

**Algorithms**: Latent Dirichlet Allocation \(LDA\)


| AWS Region | Training image and inference image registry path | 
| --- | --- | 
| us\-west\-1 | 632365934929\.dkr\.ecr\.us\-west\-1\.amazonaws\.com | 
| us\-west\-2 | 266724342769\.dkr\.ecr\.us\-west\-2\.amazonaws\.com | 
| us\-east\-1 | 766337827248\.dkr\.ecr\.us\-east\-1\.amazonaws\.com | 
| us\-east\-2 | 999911452149\.dkr\.ecr\.us\-east\-2\.amazonaws\.com | 
| ap\-northeast\-1 | 258307448986\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com | 
| ap\-northeast\-2 | 293181348795\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com | 
| ap\-south\-1 | 991648021394\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com | 
| ap\-southeast\-1 | 475088953585\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com | 
| ap\-southeast\-2 | 297031611018\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com | 
| ca\-central\-1 | 469771592824\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com | 
| eu\-central\-1 | 353608530281\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com | 
| eu\-west\-1 | 999678624901\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com | 
| eu\-west\-2 | 644912444149\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com | 
| us\-gov\-west\-1 | 226302683700\.dkr\.ecr\.us\-gov\-west\-1\.amazonaws\.com | 

**Algorithms**: XGBoost \(0\.90\-1, 0\.90\-2, 1\.0\-1\)


| AWS Region | Training image and inference image registry path | 
| --- | --- | 
| us\-west\-1 | 746614075791\.dkr\.ecr\.us\-west\-1\.amazonaws\.com | 
| us\-west\-2 | 246618743249\.dkr\.ecr\.us\-west\-2\.amazonaws\.com | 
| us\-east\-1 | 683313688378\.dkr\.ecr\.us\-east\-1\.amazonaws\.com | 
| us\-east\-2 | 257758044811\.dkr\.ecr\.us\-east\-2\.amazonaws\.com | 
| ap\-northeast\-1 | 354813040037\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com | 
| ap\-northeast\-2 | 366743142698\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com | 
| ap\-southeast\-1 | 121021644041\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com | 
| ap\-southeast\-2 | 783357654285\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com | 
| ap\-south\-1 | 720646828776\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com | 
| ap\-east\-1 | 651117190479\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com | 
| ca\-central\-1 | 341280168497\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com | 
| cn\-north\-1 | 450853457545\.dkr\.ecr\.cn\-north\-1\.amazonaws\.com\.cn | 
| cn\-northwest\-1 | 451049120500\.dkr\.ecr\.cn\-northwest\-1\.amazonaws\.com\.cn | 
| eu\-central\-1 | 492215442770\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com | 
| eu\-north\-1 | 662702820516\.dkr\.ecr\.eu\-north\-1\.amazonaws\.com | 
| eu\-west\-1 | 141502667606\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com | 
| eu\-west\-2 | 764974769150\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com | 
| eu\-west\-3 | 659782779980\.dkr\.ecr\.eu\-west\-3\.amazonaws\.com | 
| me\-south\-1 | 801668240914\.dkr\.ecr\.me\-south\-1\.amazonaws\.com | 
| sa\-east\-1 | 737474898029\.dkr\.ecr\.sa\-east\-1\.amazonaws\.com | 
| us\-gov\-west\-1 | 414596584902\.dkr\.ecr\.us\-gov\-west\-1\.amazonaws\.com | 

Use the paths and training input mode as follows:
+ To create a training job \(with a request to the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API\), specify the Docker Registry path and the training input mode for the training image\. You create a training job to train a model using a specific dataset\. 

   
+ To create a model \(with a [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request\), specify the Docker Registry path for the inference image\. Amazon SageMaker launches machine learning compute instances that are based on the endpoint configuration and deploys the model, which includes the artifacts \(the result of model training\)\.