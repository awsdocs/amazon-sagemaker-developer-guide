# Use Algorithm and Model Package Resources<a name="sagemaker-mkt-buy"></a>

You can create algorithms and model packages as resources in your Amazon SageMaker account, and you can find and subscribe to algorithms and model packages on AWS Marketplace\.

Use algorithms to:
+ Run training jobs\. For information, see [Use an Algorithm to Run a Training Job](sagemaker-mkt-algo-train.md)\.
+ Run hyperparameter tuning jobs\. For information, see [Use an Algorithm to Run a Hyperparameter Tuning Job](sagemaker-mkt-algo-tune.md)\.
+ Create model packages\. After you use an algorithm resource to run a training job or a hyperparameter tuning job, you can use the model artifacts that these jobs output along with the algorithm to create a model package\. For information, see [Create a Model Package Resource](sagemaker-mkt-create-model-package.md)\.
**Note**  
If you subscribe to an algorithm on AWS Marketplace, you must create a model package before you can use it to get inferences by creating hosted endpoint or running a batch transform job\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/mkt-buyer-workflow.png)

Use model packages to:
+ Create models that you can use to get real\-time inference or run batch transform jobs\. For information, see [Use a Model Package to Create a Model](sagemaker-mkt-model-pkg-model.md)\.
+ Create hosted endpoints to get real\-time inference\. For information, see [Deploy the Model to SageMaker Hosting Services](ex1-model-deployment.md#ex1-deploy-model)\.
+ Create batch transform jobs\. For information, see [\(Optional\) Make Prediction with Batch Transform](ex1-model-deployment.md#ex1-batch-transform)\.

**Topics**
+ [Use an Algorithm to Run a Training Job](sagemaker-mkt-algo-train.md)
+ [Use an Algorithm to Run a Hyperparameter Tuning Job](sagemaker-mkt-algo-tune.md)
+ [Use a Model Package to Create a Model](sagemaker-mkt-model-pkg-model.md)