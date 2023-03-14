# Amazon SageMaker Model Registry FAQ<a name="modelregistryfaq"></a>

Use the following FAQ items to find answers to commonly asked questions about SageMaker Model Registry\.

## Q\. How should I organize my models into model groups and model packages in the SageMaker Model Registry?<a name="collapsible-section-2mr"></a>

A model package is the actual model that is registered into the model registry as a versioned entity\. Please note there are two ways you can use model packages in SageMaker\. One is with [SageMaker Marketplace](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-marketplace.html) — these model packages are not versioned\. The other is with the SageMaker Model Registry, in which the model package *must* be versioned\. The model registry receives every new model that you retrain, gives it a version, and assigns it to a model group inside the model registry\. The following image shows an example of a model group with 25 consecutively\-versioned models\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-in-mr3.png)

## Q\. How does a model registry differ from Amazon Elastic Container Registry \(Amazon ECR\)?<a name="collapsible-section-9mr"></a>

SageMaker’s Model Registry is a metadata store for your machine learning models\. Amazon Elastic Container Registry is a repository that stores all of your containers\. Within the model registry, models are versioned and registered as model packages within model groups\. Each model package contains an Amazon S3 URI to the model files associated with the trained model and an Amazon ECR URI that points to the container used while serving the model\. 

## Q\. How do I tag model packages in the SageMaker Model Registry?<a name="collapsible-section-10mr"></a>

Model packages in the SageMaker Model Registry do not support tags—these are versioned model packages\. Instead, you can add key value pairs using `CustomerMetadataProperties`\. Model package groups in the model registry support tagging\. 